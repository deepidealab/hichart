<!DOCTYPE html>
<html>
<meta charset="utf-8">
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<meta name="viewport" content="width=device-width, initial-scale=1"><!-- Begin Jekyll SEO tag v2.8.0 -->

<head>
  <title>Make & HiChart Integration</title>
  <script src="https://code.jquery.com/jquery-1.12.4.js"></script>
  <script src="https://code.highcharts.com/highcharts.js"></script>
  <script src="https://code.highcharts.com/highcharts.js"></script>
  <script src="https://code.highcharts.com/modules/series-label.js"></script>
  <script src="https://code.highcharts.com/modules/exporting.js"></script>
  <script src="https://code.highcharts.com/modules/export-data.js"></script>
  <script src="https://code.highcharts.com/modules/accessibility.js"></script>

  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.0-beta1/dist/css/bootstrap.min.css" rel="stylesheet"
    integrity="sha384-0evHe/X+R7YkIZDRvuzKMRqM+OrBnVFBL6DOitfPri4tjfHxaWutUpFmBp4vmVor" crossorigin="anonymous">
</head>

<body>
  </br>
  <div class="container-sm">
    <input id="btnSubmit" type="submit" class="button" value="Generate Report" />
  </div>
  </div>

  <div id="container" style="height: 300px">
  </div>

  <script>
    function myFunction(p1) {
      var title = {
        text: 'Make Usage Report'
      };
      var subtitle = {
        text: 'Source: make.com'
      };
      var xAxis = {
        categories: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun',
          'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec']
      };
      var yAxis = {
        title: {
          text: 'Usage'
        }
      };

      var tooltip = {
        valueSuffix: '\xB0C'
      }
      var legend = {
        layout: 'vertical',
        align: 'right',
        verticalAlign: 'middle',
        borderWidth: 0
      };

      var series = [];

      for (let i = 0; i < p1.length; i++) {
        series.push(p1[i]);
      }

      var json = {};
      json.title = title;
      json.subtitle = subtitle;
      json.xAxis = xAxis;
      json.yAxis = yAxis;
      json.tooltip = tooltip;
      json.legend = legend;
      json.series = series;

      $('#container').highcharts(json);
    }

    $().ready(function () {
      $("#btnSubmit").click(function () {

        var endpoint = "https://hook.eu1.make.com/kfp0iavlt2egm1xkr54bftgyk3v9gpbh";
        endpoint += '?' + $.param({
              'action' : 'create_report',
              'security'  : '7846161',
              'format' : 'json'
          });

          $.ajax({
            url: endpoint,
            success: function(data) {
              holder = [];

              
              for (let i = 0; i < data.length; i++) {
                var type = data[i]['type'];
                console.log(type);

                var total_report = [];
                for (let x = 0; x < data[i].result.length; x++) {
                 temp = data[i].result[x];
                 result = temp['result'];
   
                 var matches = /\[(.*?)\]/g.exec(result);
  
                process_1 = matches[1].split(",");

                var total = 0;
                for (var u= 0; u < process_1.length; u++) {
                    total += process_1[u] << 0;
                }

                console.log(total)
                total_report.push(total);

              }


              temp = {
                name: type,
                data: total_report
              };

              holder.push(temp);

              }

              myFunction(holder);
            },
            error: function(data) {
              console.log("error");
            }
        });
      });
    });
  </script>


  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.0-beta1/dist/js/bootstrap.bundle.min.js"
    integrity="sha384-pprn3073KE6tl6bjs2QrFaJGz5/SUsLqktiwsUTF55Jfv3qYSDhgCecCxMW52nD2"
    crossorigin="anonymous"></script>
</body>

</html>