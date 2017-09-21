# Лабораторна работа №1

* ***Рисуем график с D3.js*** [ПОСМОТРЕТЬ](https://younggolden.github.io/lab1/)
![img](https://github.com/YoungGolden/YoungGolden.github.io/blob/master/img/Image%201.png)
```html
<!DOCTYPE html>
<html>
<head>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/d3/3.5.5/d3.min.js"></script> 
</head>
<style>
        .axis path, .axis line {
            fill: none;
            stroke: #000;
            shape-rendering: crispEdges;
        }
      
        .dot {
          stroke: #D11616;
          fill: #D11616;
          }
      
        path.line {
            fill: none;
            stroke-width: 1px;
        }
        .zoomOut {
            fill: #66a;
            cursor: pointer;
        }
        .zoomOutText {
            pointer-events: none;
            fill : #ccc;
        }
        .zoomOverlay {
            pointer-events: all;
            fill:none;
        }
        .band {
            fill : none;
            stroke-width: 1px;
            stroke: red;
        }
      </style>
<body>
  <div id="graph"></div>

<div>

        
        
        <script>
          var data = [];
          var bandPos = [-1, -1];
          var pos;
          var xdomain = 30;
          var ydomain = 30;
          var padding = 20;
          var colors = ["steelblue", "green"];
          
          var margin = {
            top: 40,
            right: 40,
            bottom: 50,
            left: 60
          }
          var width = 760 - margin.left - margin.right;
          var height = 450 - margin.top - margin.bottom;
          var zoomArea = {
            x1: 0,
            y1: 0,
            x2: xdomain,
            y2: ydomain
          };
          var drag = d3.behavior.drag();
      
          //data for testing
          var d1 = [
              [0.2, 16.60],
              [0.4, 16.60],
              [0.6, 16.60],
              [0.8, 16.60],
              [1.0, 16.50],
              [1.2, 16.50],
              [1.4, 16.50],
              [1.6, 16.50],
              [1.8, 16.50],
              [2.0, 16.50],
              [2.2, 16.40],
              [2.4, 16.40],
              [2.6, 16.40],
              [2.8, 16.40],
              [3.0, 16.40],
              [3.2, 16.40],
              [3.4, 16.40],
              [3.6, 16.30],
              [3.8, 16.30],
              [4.0, 16.30],
              [4.2, 16.30],
              [4.4, 16.30],
              [4.6, 16.30],
              [4.8, 16.30],
              [5.0, 16.20],
              [5.2, 16.20],
              [5.4, 16.20],
              [5.6, 16.20],
              [5.8, 16.20],
              [6.0, 16.10],
              [6.2, 16.00],
              [6.4, 16.00],
              [6.6, 15.90],
              [6.8, 15.90],
              [7.0, 15.80],
              [7.2, 15.70],
              [7.4, 15.66],
              [7.6, 15.55],
              [7.8, 15.40],
              [8.0, 15.20],
              [8.2, 15.10],
              [8.4, 14.70],
              [8.6, 14.30],
              [8.8, 13.80],
              [9.0, 13.00],
              [9.2, 12.20],
              [9.4, 11.20],
              [9.6, 9.30],
              [9.8, 7.10],
              [10.0, 5.80],
              [10.2, 4.00],
              [10.4, 2.40],
              [10.6, 1.50],
              [10.8, 1.20],
              [11.0, 1.10],
              [11.2, 1.00],
              [11.4, 0.90], 
              [11.6, 0.85],
              [11.8, 0.83],
              [12.0, 0.80],
              [12.2, 0.78],
              [12.4, 0.75],
              [12.6, 0.70],
              [12.8, 0.70],
              [13.0, 0.70],
              [13.2, 0.68],
              [13.4, 0.65],
              [13.6, 0.65],
              [13.8, 0.62],
              [14.0, 0.60],
              [14.2, 0.60],
              [14.4, 0.60],
              [14.6, 0.62],
              [14.8, 0.65],
              [15.0, 0.69],
              [15.2, 0.72],
              [15.4, 0.78],
              [15.6, 0.81],
              [15.8, 0.90],
              [16.0, 0.92],
              [16.2, 1.00],
              [16.4, 1.10],
              [16.6, 1.20],
              [16.8, 1.30],
              [17.0, 1.50],
              [17.2, 1.70],
              [17.4, 2.00],
              [17.6, 2.30],
              [17.8, 2.60],
              [18.0, 3.10],
              [18.2, 3.70],
              [18.4, 4.30],
              [18.6, 5.00],
              [18.8, 5.90],
              [19.0, 7.30],
              [19.2, 9.50],
              [19.4, 10.70],
              [19.6, 11.60],
              [19.8, 12.50],
              [20.0, 13.50],
              [20.2, 14.20],
              [20.4, 14.80],
              [20.6, 15.20],
              [20.8, 15.50],
              [21.0, 15.70],
              [21.2, 15.80],
              [21.4, 15.98],  
              [21.6, 16.07],
              [21.8, 16.12],
              [22.0, 16.20],
              [22.2, 16.22],
              [22.4, 16.28],
              [22.6, 16.31],
              [22.8, 16.35],
              [23.0, 16.39],
              [23.2, 16.40],
              [23.4, 16.40],
              [23.6, 16.41],
              [23.8, 16.43],
              [24.0, 16.45],
              [24.2, 16.45],
              [24.4, 16.46],
              [24.6, 16.48],
              [24.8, 16.50],
              [25.0, 16.50],
              [25.2, 16.50],
              [25.4, 16.50],
              [25.6, 16.50],
              [25.8, 16.50],
              [26.0, 16.50],
              [26.2, 16.50],
              [26.4, 16.50],
              [26.6, 16.50],
              [26.8, 16.50],
              [27.0, 16.50],
              [27.2, 16.50],
              [27.4, 16.50],
              [27.6, 16.50]
          ];
          var d2 = [];
      
          data.push(d1);
          data.push(d2);
          
          var svg = d3.select("body").append("svg")
            .attr("width", width + margin.left + margin.right)
            .attr("height", height + margin.top + margin.bottom)
            .append("g")
            .attr("transform", "translate(" + margin.left + "," + margin.top + ")");
      
      
          var x = d3.scale.linear()
            .range([0, width]).domain([0, xdomain]);
      
          var y = d3.scale.linear()
            .range([height, 0]).domain([0, ydomain]);
      
          //Создаем ось X
          var xAxis = d3.svg.axis()
            .scale(x)
            .orient("bottom");
      
          //Создаем ось Y
          var yAxis = d3.svg.axis()
            .scale(y)
            .orient("left");
   /*
          svg.selectAll("circle")
            .data(d1)
            .enter()
            .append("circle")
            .attr("class", "dot")
            .attr("cx", function(d) {
              return x(d[0]);
            })
            .attr("cy", function(d) {
              return y(d[1]);
            })
            .attr("r", 1);
    */     
          //Соединяем точки прямой, либо интерполируем
          var line = d3.svg.line()
            .interpolate("basis") // интерполяция.
            .x(function(d) {
            return x(d[0]);
            })
            .y(function(d) {
            return y(d[1]);
            });
            
          
          var band = svg.append("rect")
            .attr("width", 0)
            .attr("height", 0)
            .attr("x", 0)
            .attr("y", 0)
            .attr("class", "band");
      
            svg.append("g")
            .attr("class", "x axis")
            .call(xAxis)
            .attr("transform", "translate(0," + height + ")")
            .append("text")
                .attr("fill", "#000")
                .attr("x", width)
                .attr("y", -6)
                .attr("text-anchor", "end")
                .text("X(см)");
      
            svg.append("g")
            .attr("class", "y axis")
            .call(yAxis)
            .append("text")
                .attr("fill", "#000")
                .attr("transform", "rotate(-90)")
                .attr("y", 6)
                .attr("dy", "0.71em")
                .attr("text-anchor", "end")
                .text("Y(см)");
      
          svg.append("clipPath")
            .attr("id", "clip")
            .append("rect")
            .attr("width", width)
            .attr("height", height);
      
          for (idx in data) {
            svg.append("path")
              .datum(data[idx])
              .attr("class", "line line" + idx)
              .attr("clip-path", "url(#clip)")
              .style("stroke", colors[idx])
              .attr("d", line);
          }
      
          var zoomOverlay = svg.append("rect")
            .attr("width", width - 10)
            .attr("height", height)
            .attr("class", "zoomOverlay")
            .call(drag);
      
          var zoomout = svg.append("g");
      
          zoomout.append("rect")
            .attr("class", "zoomOut")
            .attr("width", 75)
            .attr("height", 40)
            .attr("x", -12)
            .attr("y", height + (margin.bottom - 20))
            .on("click", function() {
              zoomOut();
            });
      
          zoomout.append("text")
            .attr("class", "zoomOutText")
            .attr("width", 75)
            .attr("height", 30)
            .attr("x", -10)
            .attr("y", height + (margin.bottom - 5))
            .text("Zoom Out");
      
          zoom();
      
          drag.on("dragend", function() {
            var pos = d3.mouse(this);
            var x1 = x.invert(bandPos[0]);
            var x2 = x.invert(pos[0]);
      
            if (x1 < x2) {
              zoomArea.x1 = x1;
              zoomArea.x2 = x2;
            } else {
              zoomArea.x1 = x2;
              zoomArea.x2 = x1;
            }
      
            var y1 = y.invert(pos[1]);
            var y2 = y.invert(bandPos[1]);
      
            if (x1 < x2) {
              zoomArea.y1 = y1;
              zoomArea.y2 = y2;
            } else {
              zoomArea.y1 = y2;
              zoomArea.y2 = y1;
            }
      
            bandPos = [-1, -1];
      
            d3.select(".band").transition()
              .attr("width", 0)
              .attr("height", 0)
              .attr("x", bandPos[0])
              .attr("y", bandPos[1]);
      
            zoom();
          });
      
          drag.on("drag", function() {
      
            var pos = d3.mouse(this);
      
            if (pos[0] < bandPos[0]) {
              d3.select(".band").
              attr("transform", "translate(" + (pos[0]) + "," + bandPos[1] + ")");
            }
            if (pos[1] < bandPos[1]) {
              d3.select(".band").
              attr("transform", "translate(" + (pos[0]) + "," + pos[1] + ")");
            }
            if (pos[1] < bandPos[1] && pos[0] > bandPos[0]) {
              d3.select(".band").
              attr("transform", "translate(" + (bandPos[0]) + "," + pos[1] + ")");
            }
      
            //set new position of band when user initializes drag
            if (bandPos[0] == -1) {
              bandPos = pos;
              d3.select(".band").attr("transform", "translate(" + bandPos[0] + "," + bandPos[1] + ")");
            }
      
            d3.select(".band").transition().duration(1)
              .attr("width", Math.abs(bandPos[0] - pos[0]))
              .attr("height", Math.abs(bandPos[1] - pos[1]));
          });
      
          function zoom() {
            //recalculate domains
            if (zoomArea.x1 > zoomArea.x2) {
              x.domain([zoomArea.x2, zoomArea.x1]);
            } else {
              x.domain([zoomArea.x1, zoomArea.x2]);
            }
      
            if (zoomArea.y1 > zoomArea.y2) {
              y.domain([zoomArea.y2, zoomArea.y1]);
            } else {
              y.domain([zoomArea.y1, zoomArea.y2]);
            }
      
            //update axis and redraw lines
            var t = svg.transition().duration(750);
            t.select(".x.axis").call(xAxis);
            t.select(".y.axis").call(yAxis);
      
            t.selectAll(".line").attr("d", line); 
          }
      
          var zoomOut = function() {
            x.domain([0, xdomain]);
            y.domain([0, ydomain]);
      
            var t = svg.transition().duration(750);
            t.select(".x.axis").call(xAxis);
            t.select(".y.axis").call(yAxis);
            t.selectAll(".line").attr("d", line);     
          }
          
        </script>
  </body>
  </html>
```
# Пишем код на с++ для нахождения y по x

```cpp
#include <fstream>
#include <iostream>
#include <cmath>

using namespace std;

void fup (double x, double x_1, double x_2, double y_1, double y_2){//функция вычисления y
    double y;
    y=((x-x_1)*(y_2-y_1)/(x_2-x_1))+y_1;
    cout << y << endl;
}

int main(){

    double x;
    double y_1,y_2,x_1,x_2;

    cout << "x>>>" << endl;

        cin >> x;
        if(x < 0 || x > 160){
            cout << "Errore x < 0 || x > 160" << endl;
            return 0;
        };

    ifstream coord;
    coord.open("text.txt");

    if(!coord){
        cout << "Errore" <<endl;
        return 0;
        }
    else{
        f >> x_2 >> y_2;
        while(!coord.eof()) {
            if(x_2 < x){
            x_1 = x_2;
            y_1 = y_2;
            coord >> x_2 >> y_2;
            }
            else if(x_2 == x){
                cout << y_2 << endl;
                coord.close();
                return 0;
            }
            else {
                fup (x, x_1, x_2, y_1, y_2);
                coord.close();
                return 0;
            }
        }
    }
    coord.close();
    return 0;
}
```
