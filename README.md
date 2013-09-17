bar-chart
=========

Bar chart lesson. Week 2

<!-- shamelessly stolen from scott murray: http://alignedleft.com/tutorials -->
<!DOCTYPE html>
<html lang="en">
	<style type="text/css">
	svg {
		padding-right: 50px;
	}
	.bar{
		fill:#ddd;
	}

	.g-minor-highlight {
		fill: #999;
	}

	.g-ESPN {
		fill:orange;
		font-family: arial;
	}

	.g-labels {
		display:none;
		text-anchor:end;
		font-family: arial;
	}

	.g-minor-highlight-label {
		fill:#999;
		display:block;
		font-size: 10px;
		font-family: arial;

	}

	.text{
		font-family: arial;
		font-size: 13px;

	}
	.yAxis path{
		fill: none;

	}
	.yAxis line{
		fill: none;
		stroke: black;
		shape-rendering: crispEdges;

	}
	.yAxis text{
		font-family: Arial;
		font-size: 10px;
	}
	.xAxis path,
	.xAxis line{
		fill: none;
		stroke: black;
		shape-rendering:crispEdges;
	}

	.ESPN-label {
		font-family: arial;
		text-anchor: middle;
		font-size: 14px;
		font-weight: bold;
	}


	h1{
		font-family: arial;
	}
	h2, p{
		font-family: arial;
	}

	html, body {
		font-family: arial;
	}

	</style>

    <head>
        <meta charset="utf-8">
        <title>D3 Test</title>
        <script src="http://d3js.org/d3.v3.min.js" charset="utf-8"></script>
    </head>
    <body>
        <script type="text/javascript">
            // Your beautiful D3 code will go here
			d3.select("body").append("h1").text("An expensive outlier")
			var margin = {top: 20, right: 10, bottom: 20, left: 10};

			var width = 1050 - margin.left - margin.right,
				height = 450 - margin.top - margin.bottom;

			var svg = d3.select("body").append("svg")
			    .attr("width", width + margin.left + margin.right)
			    .attr("height", height + margin.top + margin.bottom)
			    .append("g")
			    .attr("transform", "translate(" + margin.left + "," + margin.top + ")");

					var networks = ["Comedy Central","TNT", "C-SPAN", "The Weather Channel", "MSNBC", "Bravo", "TBS", "ESPN2"]

					d3.csv("subscription-prices.csv", function(err, prices) { 
						prices.forEach(function(d) {
						    // recasts d.2013 as a number, not a string
						    d.X2013 = +d.X2013;
						})

						prices.sort(function(a,b) {
						    return a.X2013 - b.X2013;
						});

						console.log(prices)

							var y = d3.scale.linear()
							    .domain([0,6])
							    .range([0,height]);

							var x = d3.scale.linear()
							    .domain([0,prices.length])
							    .range([0,width]);

							var bar = svg.selectAll(".bar")
							   		.data(prices)
						    		.enter().append("rect")
									.attr("height", function(d) { return y(d.X2013); })
							 		.attr("width", 4)
									.attr("y", function(d) { return height - y(d.X2013); })
							 		.attr("x", function(d, i) { return 5.30 * i})
						    		.attr("class", "bar")
									.classed("g-ESPN", function(d) { return d.Network == "ESPN"; })
								// .classed("g-minor-highlight", function(d) { return (d.Network == "TNT") || (d.Network == "Comedy Central"); })
									.classed("g-minor-highlight", function(d) { return networks.indexOf(d.Network) >= 0; });

							var labels = svg.selectAll(".g-labels")
								.data(prices)
								.enter().append("text")
								.attr("x", function(d,i) { return 5.30*i})
								.attr("y", function(d) { return height - y(d.X2013) - 14 })
								.text(function(d) { return d.Network + " ($"+ d.X2013+")"; })
								.attr("class", "g-labels")
								.classed("g-minor-highlight-label", function(d) { return networks.indexOf(d.Network) >= 0; });


								var dollars = d3.format("$")
								y.domain ([0, 6]) 
								.range([height, 0])


								var yAxis = d3.svg.axis()
									//.orient(bottom)
									.scale(y)
									//.tickFormat(f)
									.ticks(5)
									.orient ("right")
									.tickFormat(dollars)
									.tickValues([1, 2, 3, 4, 5]);


								svg.append("g")
									.attr("class","yAxis")
									.call(yAxis)
									.attr("transform", "translate(" + width + ", 0)");
									//.axis.tickFormat (Function(d) {return "$" + d;});

							//My xAxis is at at the top of the page despite my attempts to transform... and when I thought I did, "ESPN" would disappear//
								var xPadding = 400

								var xAxis = d3.svg.axis()
									.scale(x)
									.orient("bottom")
									.ticks(0)
									.tickSize(0);
								svg.append("g")
									.attr("class", "xAxis")
									.attr("transform", "translate(0" + (height - xPadding) + ")")
									.call(xAxis);

									//var bottomyAxis = d3.svg.axis()
									//   .scale (y)
									//   .ticketValues([0])
									//   .tickSize (width)
									//   .orient ("right");



								// [1, 2, 3].indexOf(3) >= 0;
								// https://gist.github.com/mbostock/3934356

								svg.append("text")
							 		.attr("class", "g-label")
								 	.attr("x", 1030)
									.attr("y", 30)
								  	.text("ESPN")
									//.data(dataset)
									//.enter()
									//.append ("p")
									//.text( function(d) {return "$5.54"});
									//



								// 								// 								
								// 								svg.append("text")
								// 								    .attr("class", "g-label")
								// 								    .attr("x", 430)
								// 								    .attr("y", 20)
								// 								    .text("C-SPAN");
								// 								
								// 									svg.append("text")
								// 									    .attr("class", "g-label")
								// 									    .attr("x", 430)
								// 									    .attr("y", 20)
								// 									    .text("Comedy Central");
								// 									
								// 										svg.append("text")
								// 										    .attr("class", "g-label")
								// 										    .attr("x", 430)
								// 										    .attr("y", 20)
								// 										    .text("The Weather Channel");
								// 										
								// 											svg.append("text")
								// 											    .attr("class", "g-label")
								// 											    .attr("x", 430)
								// 											    .attr("y", 20)
								// 											    .text("MSNBC");
								// 											
								// 												svg.append("text")
								// 												    .attr("class", "g-label")
								// 												    .attr("x", 430)
								// 												    .attr("y", 20)
								// 												    .text("Bravo");
								// 											
								// 												svg.append("text")
								// 													    .attr("class", "g-label")
								// 													    .attr("x", 430)
								// 													    .attr("y", 20)
								// 													    .text("TBS");
								// 													
								// 													svg.append("text")
								// 														    .attr("class", "g-label")
								// 														    .attr("x", 495)
								// 														    .attr("y", 20)
								// 														    .text("ESPN2");
								// 				
								// 															svg.append("text")
								// 																    .attr("class", "g-label")
								// 																    .attr("x", 500)
								// 																    .attr("y", 20)
								// 																    .text("TNT");


					 }); // End of the CSV




        </script>

    
</body>
<h2>Questions</h2>
<p>1. What is the average monthly price to consumers as compared to ESPN's monthly expenses? </p>
<p>2. What does the bar chart look like when ESPN is taken out of the equation? </p>
<p>3. How can the majority of the networks keep the price under $1 per month for consumers? </p>
</html>
