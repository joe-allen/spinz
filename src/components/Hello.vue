<template>
  <div class="hello">
    <div id="chart">
      <img id="logo" src="http://138.197.2.49/_nuxt/img/logo.f9d04ae.png">
    </div>
    <div id="question"><h1></h1></div>
  </div>
</template>

<script>

export default {
  name: 'hello',
  data () {
    return {
    }
  },
  mounted () {

    // This game is thanks to: https://code.sololearn.com/WD8GprR5hozY/#html

    var padding = {top:20, right:40, bottom:0, left:0},
        w = 500 - padding.left - padding.right,
        h = 520 - padding.top  - padding.bottom,
        r = Math.min(w, h)/2,
        rotation = 0,
        oldrotation = 0,
        picked = 100000,
        oldpick = [],
        color = d3.scale.category20c();//category20()
        //randomNumbers = getRandomNumbers();
    //http://osric.com/bingo-card-generator/?title=HTML+and+CSS+BINGO!&words=padding%2Cfont-family%2Ccolor%2Cfont-weight%2Cfont-size%2Cbackground-color%2Cnesting%2Cbottom%2Csans-serif%2Cperiod%2Cpound+sign%2C%EF%B9%A4body%EF%B9%A5%2C%EF%B9%A4ul%EF%B9%A5%2C%EF%B9%A4h1%EF%B9%A5%2Cmargin%2C%3C++%3E%2C{+}%2C%EF%B9%A4p%EF%B9%A5%2C%EF%B9%A4!DOCTYPE+html%EF%B9%A5%2C%EF%B9%A4head%EF%B9%A5%2Ccolon%2C%EF%B9%A4style%EF%B9%A5%2C.html%2CHTML%2CCSS%2CJavaScript%2Cborder&freespace=true&freespaceValue=Web+Design+Master&freespaceRandom=false&width=5&height=5&number=35#results
    var data = [
        {"label":"Try again",  "value":1,  "question":"Sorry, better luck next time!"}, // padding
        {"label":"75% OFF", "value":1, "question":"You've won 75% off your next visit at Camp Bow Wow. See your inbox for details!"}, // <style>
        {"label":"Try again",  "value":1,  "question":"Sorry, better luck next time!"}, //color
        {"label":"FREE TRIM", "value":1, "question":"You've won a free haircut for your pet at Camp Bow Wow. See your inbox for details!"}, // <style>
        {"label":"FREE CLIPPING", "value":1, "question":"You've won a free nail clipping for your pet at Camp Bow Wow. See your inbox for details!"}, // <style>
        {"label":"FREE GROOM", "value":1, "question":"You've won a free grooming for your pet at Camp Bow Wow. See your inbox for details!"}, // <style>
        {"label":"25% OFF", "value":1, "question":"You've won 25% off your next visit at Camp Bow Wow. See your inbox for details!"}, // <style>
        {"label":"50% OFF", "value":1, "question":"You've won 50% off your next visit at Camp Bow Wow. See your inbox for details!"}, // <style>
        {"label":"Try again",  "value":1,  "question":"Sorry, better luck next time!"}, // padding
        {"label":"50% OFF", "value":1, "question":"You've won 50% off your next visit at Camp Bow Wow. See your inbox for details!"}, // <style>
        {"label":"$25 Home Depot Gift Card", "value":1, "question":"Check you email to redeem a Home Depot gift card for $25!"}, // HTML
        {"label":"2 NIGHT STAY", "value":1, "question":"You've won a free nights stay at Camp Bow Wow ($200 value!). See your inbox for details!"}, // <style>
        {"label":"Try again", "value":1, "question":"Sorry, better luck next time!"}, //< >
        {"label":"CampBowWow HAT", "value":1, "question":"Thanks for playing. Here's a free hat!"}, //<p>
        {"label":"Try again", "value":1, "question":"Sorry, better luck next time!"}, //< >
        {"label":"Try again", "value":1, "question":"Sorry, better luck next time!"}, // { }
        {"label":"CampBowWow T-SHIRT", "value":1, "question":"Thanks for playing. Here's a free T-shirt!"}, //<p>
        {"label":"CampBowWow LEASH", "value":1, "question":"Thanks for playing. Here's a leash!"}, //<p>
        {"label":"Try again", "value":1, "question":"Sorry, better luck next time!"}, //<head>
        {"label":"Try again", "value":1, "question":"Sorry, better luck next time!"}, // colon
        {"label":"1 NIGHT STAY", "value":1, "question":"You've won a free nights stay at Camp Bow Wow ($100 value!). See your inbox for details!"}, // <style>
        {"label":"FREE DOGGIE BATH", "value":1, "question":"Check you email for a free dog bath on your next visit!"}, // .html
        {"label":"$25 Amazon Gift Card", "value":1, "question":"Check you email to redeem an Amazon gift card for $25!"}, // HTML
        {"label":"Try again", "value":1, "question":"Sorry, better luck next time!"}, // CSS
        {"label":"Try again", "value":1, "question":"Sorry, better luck next time!"}, // JavaScript
        {"label":"AMX $40", "value":1, "question":"Check you email to redeem an American Express credit card for $40!"}, // border
        {"label":"Try again", "value":1, "question":"Sorry, better luck next time!"}, // CSS
        {"label":"FRANK'S BEANS", "value":1, "question":"YOUR SIGNIFICANT OTHER IS THE REAL WINNER TONIGHT! CONGRATS!!"} //comma
    ];
    var svg = d3.select('#chart')
        .append("svg")
        .data([data])
        .attr("width",  w + padding.left + padding.right)
        .attr("height", h + padding.top + padding.bottom);
    var container = svg.append("g")
        .attr("class", "chartholder")
        .attr("transform", "translate(" + (w/2 + padding.left) + "," + (h/2 + padding.top) + ")");
    var vis = container
        .append("g");

    var pie = d3.layout.pie().sort(null).value(function(d){return 1;});
    // declare an arc generator function
    var arc = d3.svg.arc().outerRadius(r);
    // select paths, use arc generator to draw
    var arcs = vis.selectAll("g.slice")
        .data(pie)
        .enter()
        .append("g")
        .attr("class", "slice");

    arcs.append("path")
        .attr("fill", function(d, i){ return color(i); })
        .attr("d", function (d) { return arc(d); });
    // add the text
    arcs.append("text").attr("transform", function(d){
            d.innerRadius = 0;
            d.outerRadius = r;
            d.angle = (d.startAngle + d.endAngle)/2;
            return "rotate(" + (d.angle * 180 / Math.PI - 90) + ")translate(" + (d.outerRadius -10) +")";
        })
        .attr("text-anchor", "end")
        .text( function(d, i) {
            return data[i].label;
        });

    // use wheel as spin event
    container.on("click", spin);

    // use logo as spin event also
    var logo = document.getElementById('logo');
    logo.addEventListener('click', spin)


    function spin(d){

        container.on("click", null);
        //all slices have been seen, all done
        console.log("OldPick: " + oldpick.length, "Data length: " + data.length);
        if(oldpick.length == data.length){
            console.log("done");
            container.on("click", null);
            return;
        }
        var  ps       = 360/data.length,
             pieslice = Math.round(1440/data.length),
             rng      = Math.floor((Math.random() * 1440) + 360);

        rotation = (Math.round(rng / ps) * ps);

        picked = Math.round(data.length - (rotation % 360)/ps);
        picked = picked >= data.length ? (picked % data.length) : picked;
        if(oldpick.indexOf(picked) !== -1){
            d3.select(this).call(spin);
            return;
        } else {
            oldpick.push(picked);
        }
        rotation += 90 - Math.round(ps/2);
        vis.transition()
            .duration(4000)
            .attrTween("transform", rotTween)
            .each("end", function(){
                //mark question as seen
                // d3.select(".slice:nth-child(" + (picked + 1) + ") path")
                //     .attr("fill", "#111");
                //populate question
                d3.select("#question h1")
                    .text(data[picked].question);
                oldrotation = rotation;

                container.on("click", spin);
            });
    }
    //make arrow
    svg.append("g")
        .attr("transform", "translate(" + (w + padding.left + padding.right) + "," + ((h/2)+padding.top) + ")")
        .append("path")
        .attr("d", "M-" + (r*.0826087) + ",0L0," + (r*.05) + "L0,-" + (r*.05) + "Z")
        .style({"fill":"#b01538"});
    //draw spin circle
    container.append("circle")
        .attr("cx", 0)
        .attr("cy", 0)
        .attr("r", 60)
        .style({"fill":"white","cursor":"pointer"});
    //spin text
    // container.append("text")
    //     .attr("x", 0)
    //     .attr("y", 15)
    //     .attr("text-anchor", "middle")
    //     .text("SPINNER")
    //     .style({"font-weight":"bold", "font-size":"30px"});


    function rotTween(to) {
      var i = d3.interpolate(oldrotation % 360, rotation);
      return function(t) {
        return "rotate(" + i(t) + ")";
      };
    }


    function getRandomNumbers(){
        var array = new Uint16Array(1000);
        var scale = d3.scale.linear().range([360, 1440]).domain([0, 100000]);
        if(window.hasOwnProperty("crypto") && typeof window.crypto.getRandomValues === "function"){
            window.crypto.getRandomValues(array);
            console.log("works");
        } else {
            //no support for crypto, get crappy random numbers
            for(var i=0; i < 1000; i++){
                array[i] = Math.floor(Math.random() * 100000) + 1;
            }
        }
        return array;
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #35495E;
}

#chart {
  display: flex;
  justify-content: center;
  align-items: center;
}
#logo {
  position: absolute;
  width: 130px;
  padding-top: 40px;
  margin-left: -19px;
}
#logo:hover {
  cursor: pointer;
}
#question {
  padding: 0 50px;
}

/*MEDIA QUERIES*/
@media screen and (max-width: 500px) {
  #chart {
    margin-left: -500px;
  }
  #logo {
    right: 50px;
  }
}
@media screen and (max-width: 360px) {
  #logo {
    right: 30px;
  }
}
</style>
