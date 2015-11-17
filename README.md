# Readme

#### d3.js Load JSON from URI

~~~
// Load Data from /data.json URL
d3.json("data.json", function(error, root) {

    var nodes = cluster.nodes(root);
    var links = cluster.links(nodes);

    console.debug('All Nodes(所有的節點總數):', nodes, nodes.length);
    console.debug('All Links(所有點跟點之間的線段總數):', links, links.length);

    var link = svg.selectAll(".link")
        .data(links)
        .enter()
        .append("path")
        .attr("class", "link")
        .attr("d", diagonal);

    var node = svg.selectAll(".node")
        .data(nodes)
        .enter()
        .append("g")
        .attr("class", "node")
        .attr("transform", function(d) {
            return "translate(" + d.y + "," + d.x + ")";
        })

    node.append("circle")
        .attr("r", 4.5);

    node.append("text")
        .attr("dx", function(d) {
            return d.children ? -8 : 8;
        })
        .attr("dy", 3)
        .style("text-anchor", function(d) {
            return d.children ? "end" : "start";
        })
        .text(function(d) {
            return d.name;
        });
});
~~~

#### Demo
![Alt text](https://raw.githubusercontent.com/scott1028/D3.JS-Study/master/sample01.jpg "D3.JS View")
