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
  <div class="container-sm">
    <input id="btnSubmit" type="submit" class="button" value="Generate Report" />
  </div>

  <div id="container" style="height: 300px">
  </div>

  <script>
    function generator(data, categories) {
      console.log(data);
       console.log(categories);
      var title = {
        text: 'Make Usage Report'
      };
      var subtitle = {
        text: 'Source: make.com'
      };
      var xAxis = {
        categories: categories
      };
      var yAxis = {
        title: {
          text: 'Usage'
        }
      };


      var legend = {
        layout: 'vertical',
        align: 'right',
        verticalAlign: 'middle',
        borderWidth: 0
      };

      var series = [];


      for (let i = 0; i < data.length; i++) {
        series.push(data[i]);
        console.log(data[i]);
      }

      var json = {};
      json.title = title;
      json.subtitle = subtitle;
      json.xAxis = xAxis;
      json.yAxis = yAxis;
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
            
              categories = [];
              holder = [];

              operations_data = [];
              duration_data = [];
              transfer_data = [];

              console.log(data.length)

              
              for (let i = 0; i < data.length; i++) {
                var type = data[i]['f'];
                categories.push(type[3]['v'])
              }

              for (let i = 0; i < data.length; i++) {
                var type = data[i]['f'];
                operations_data.push(Number(type[1]['v']))
              }

              for (let i = 0; i < data.length; i++) {
                var type = data[i]['f'];
                duration_data.push(Number(type[2]['v']))
              }


              for (let i = 0; i < data.length; i++) {
                var type = data[i]['f'];
                transfer_data.push(Number(type[3]['v']))
              }

              for (let i = 0; i < 3; i++) {
               prepare = {
                name: "",
                data: []
              };

                switch(i) {
                  case 0:
                    prepare['name'] = "operations";
                    prepare['data'] = operations_data
                    break;
                  case 1:
                    prepare['name'] = "duration";
                    prepare['data'] = duration_data
                    break;
                  case 2:
                    prepare['name'] = "transfer";
                    prepare['data'] = transfer_data
                    break;
                  default:
                    // code block
                }

                holder.push(prepare)


              }

              generator(holder, categories);
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