# Readme

#### d3.js Load JSON from URI

- sample01.html: 最簡單的範例！
- sample02.html: 搭配 AngularJS 的範例！
- sample01.html: 更換 .children .name 的 FieldName！
- ref: http://nlpviz.bpodgursky.com/home
- ref: http://bl.ocks.org/mbostock/999346 (樹狀圖同 Layer 對齊問題)

~~~
// 讓 node & link 的繪製點移動
// Add entering nodes in the parent’s old position.
node.enter().append("circle")
    .attr("class", "node")
    .attr("r", 4)
    .attr("cx", function(d) { return d.parent.px; })
    .attr("cy", function(d) { return d.parent.py; });

// Add entering links in the parent’s old position.
link.enter().insert("path", ".node")
    .attr("class", "link")
    .attr("d", function(d) {
        var o = {x: d.source.px, y: d.source.py};
        return diagonal({source: o, target: o});
    });
~~~

# Usage

- D3 每個繪製物件的屬性都可以透過 Func 來設定例如 SVG 其中一個 Text Node！

~~~
node.append("text")
    .attr("dx", function(d) {
        return d.children ? -8 : 8;
    })
    .attr("dy", 7)
    .style("text-anchor", function(d) {
        return d.children ? "end" : "start";
    })
    .attr('fill', function(d){   // d 就是這一筆的 Node Json Object，可經由 cluster.nodes(root) 得到每一個 Node Json Object！
        return (
            (d.id === id)
            ?
            'red'
            :
            '#555555'
        );
    })
    .style("font-size","16px")
    .text(function(d) {
        return d.dealerId + '-' + d.dealerName + (
            (d.id === id)
            ?
            '( * )'
            :
            ''
        );
    });
~~~

Sample#1

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
