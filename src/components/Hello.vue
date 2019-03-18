<template>
  <div class="hello">
    <!-- <div v-show="question.length !== 0"><h1 id="result">{{question}}</h1></div> -->
    <div id="prize">
      <h1 id="result"><span style="color:#b01538">{{prizeLabel}}</span><br>{{prizeDesc}}</h1>
      <button class="spin button" id="button" @click="spinAgain">Spin Again</button>
    </div>

    <div id="chart">
      <img id="logo" src="https://1p4xnw9vz583g92k7249hwjj-wpengine.netdna-ssl.com/wp-content/uploads/2016/11/CBW-logo2-300dpi-cymk.jpg">
    </div>
  </div>
</template>

<script>

import anime from 'animejs'

export default {
  name: 'hello',
  data () {
    return {
      prizeDesc: '',
      prizeLabel: '',
      resultContainer: '',
      chart: '',
      button: '',
      prize: ''
    }
  },
  mounted () {

    this.resultContainer = document.getElementById('result');
    this.prize = document.getElementById('prize');
    this.button = document.getElementById('button');
    this.chart = document.getElementById('chart');

    // This game is thanks to: https://code.sololearn.com/WD8GprR5hozY/#html
    // And this for the svg wheel border: http://jsfiddle.net/odiseo/4xk58/4/

    var padding = {top:20, right:40, bottom:0, left:0},
        w = 540 - padding.left - padding.right,
        h = 560 - padding.top  - padding.bottom,
        r = Math.min(w, h)/2,
        rotation = 0,
        oldrotation = 0,
        picked = 100000,
        oldpick = [],
        color = d3.scale.category20c();//category20()
        //randomNumbers = getRandomNumbers();
        //http://osric.com/bingo-card-generator/?title=HTML+and+CSS+BINGO!&words=padding%2Cfont-family%2Ccolor%2Cfont-weight%2Cfont-size%2Cbackground-color%2Cnesting%2Cbottom%2Csans-serif%2Cperiod%2Cpound+sign%2C%EF%B9%A4body%EF%B9%A5%2C%EF%B9%A4ul%EF%B9%A5%2C%EF%B9%A4h1%EF%B9%A5%2Cmargin%2C%3C++%3E%2C{+}%2C%EF%B9%A4p%EF%B9%A5%2C%EF%B9%A4!DOCTYPE+html%EF%B9%A5%2C%EF%B9%A4head%EF%B9%A5%2Ccolon%2C%EF%B9%A4style%EF%B9%A5%2C.html%2CHTML%2CCSS%2CJavaScript%2Cborder&freespace=true&freespaceValue=Web+Design+Master&freespaceRandom=false&width=5&height=5&number=35#results

    var data = [
        {"label":"",  "value":1,  "question":"Sorry, you did not win, but check your email for other cool opportunities!"}, // padding
        {"label":"75% OFF", "value":1, "question":"You've won 75% off your next visit at Camp Bow Wow. See your inbox for details!"}, // <style>
        {"label":"",  "value":1,  "question":"Sorry, you did not win, but check your email for other cool opportunities!"}, //color
        {"label":"FREE TRIM", "value":1, "question":"You've won a free haircut for your pet at Camp Bow Wow. See your inbox for details!"}, // <style>
        {"label":"",  "value":1,  "question":"Sorry, you did not win, but check your email for other cool opportunities!"}, // padding
        {"label":"FREE GROOM", "value":1, "question":"You've won a free grooming for your pet at Camp Bow Wow. See your inbox for details!"}, // <style>
        {"label":"25% OFF", "value":1, "question":"You've won 25% off your next visit at Camp Bow Wow. See your inbox for details!"}, // <style>
        {"label":"",  "value":1,  "question":"Sorry, you did not win, but check your email for other cool opportunities!"}, // padding
        {"label":"2 NIGHT STAY", "value":1, "question":"You've won a free nights stay at Camp Bow Wow ($200 value!). See your inbox for details!"}, // <style>
        {"label":"50% OFF", "value":1, "question":"You've won 50% off your next visit at Camp Bow Wow. See your inbox for details!"}, // <style>
        {"label":"", "value":1, "question":"Sorry, you did not win, but check your email for other cool opportunities!"}, // HTML
        {"label":"", "value":1, "question":"Sorry, you did not win, but check your email for other cool opportunities!"}, // <style>
        {"label":"FREE CLIPPING", "value":1, "question":"You've won a free nail clipping for your pet at Camp Bow Wow. See your inbox for details!"}, // <style>
        {"label":"", "value":1, "question":"Sorry, you did not win, but check your email for other cool opportunities!"}, //<p>
        {"label":"", "value":1, "question":"Sorry, you did not win, but check your email for other cool opportunities!"}, //< >
        {"label":"", "value":1, "question":"Sorry, you did not win, but check your email for other cool opportunities!"}, // { }
        {"label":"CampBowWow T-SHIRT", "value":1, "question":"Thanks for playing. Here's a free T-shirt!"}, //<p>
        {"label":"CampBowWow LEASH", "value":1, "question":"Thanks for playing. Here's a leash!"}, //<p>
        {"label":"", "value":1, "question":"Sorry, you did not win, but check your email for other cool opportunities!"}, //<head>
        {"label":"", "value":1, "question":"Sorry, you did not win, but check your email for other cool opportunities!"}, // colon
        {"label":"1 NIGHT STAY", "value":1, "question":"You've won a free nights stay at Camp Bow Wow ($100 value!). See your inbox for details!"}, // <style>
        {"label":"FREE DOGGIE BATH", "value":1, "question":"Check you email for a free dog bath on your next visit!"}, // .html
        {"label":"", "value":1, "question":"Sorry, you did not win, but check your email for other cool opportunities!"}, // HTML
        {"label":"", "value":1, "question":"Sorry, you did not win, but check your email for other cool opportunities!"}, // CSS
        {"label":"50% OFF", "value":1, "question":"You've won 50% off your next visit at Camp Bow Wow. See your inbox for details!"}, // <style>
        {"label":"", "value":1, "question":"Sorry, you did not win, but check your email for other cool opportunities!"}, // border
        {"label":"", "value":1, "question":"Sorry, you did not win, but check your email for other cool opportunities!"}, // CSS
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

    var center = 200;
    var outerRadius = 265;
    var innerRadius = 100;

    var arcOutter = d3.svg.arc()
        .innerRadius(outerRadius - 15)
        .outerRadius(outerRadius);

    // var arcPhantom = d3.svg.arc()
    //     .innerRadius(0)
    //     .outerRadius(outerRadius + 20);

    //Set up outter arc groups
    var outterArcs = svg.selectAll("g.outter-arc")
        .data(pie(data))
        .enter()
        .append("g")
        .attr("class", "outter-arc")
        .attr("transform", "translate(" + (w/2 + padding.left) + "," + (h/2 + padding.top) + ")");

    //Set up phantom arc groups
    // var phantomArcs = svg.selectAll("g.phantom-arc")
    //     .data(pie(data))
    //     .enter()
    //     .append("g")
    //     .attr("class", "phantom-arc")
    //     .attr("transform", "translate(" + (w/2 + padding.left) + "," + (h/2 + padding.top) + ")");

    //Draw outter arc paths
    outterArcs.append("path")
        .attr("fill", '#b01538')
        .attr("d", arcOutter).style('stroke', '#b01538')
        .style('stroke-width', 0);

    //Draw phantom arc paths
    // phantomArcs.append("path")
    //     .attr("fill", 'white')
    //     .attr("fill-opacity", 0.1)
    //     .attr("d", arcPhantom).style('stroke', 'white')
    //     .style('stroke-width', 1);

    arcs.append("path")
        .attr("fill", function(d, i){ return color(i); })
        .attr("d", function (d) { return arc(d); });

        let component = this;
    // add the text
    arcs.append("text").attr("transform", function(d){
            d.innerRadius = 0;
            d.outerRadius = r;
            d.angle = (d.startAngle + d.endAngle)/2;
            return "rotate(" + (d.angle * 180 / Math.PI - 90) + ")translate(" + (d.outerRadius -10) +")";
        })
        .attr("text-anchor", "end")
        .attr("y", "4") // vertically sets the text w/in slice
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

                component.showResultDetail(data[picked]);
                //mark question as seen
                // d3.select(".slice:nth-child(" + (picked + 1) + ") path")
                //     .attr("fill", "#111");
                //populate question

                // d3.select("#question h1")
                //     .text(data[picked].question);

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
  },
  methods: {
    showResultDetail: function (result) {

      this.prizeLabel = result.label;
      this.prizeDesc = result.question;

      this.resultContainer.style.backgroundColor = '#fff';
      this.prize.style.zIndex = 1;
      this.chart.style.backgroundColor = '#fff';
      this.resultContainer.style.transform = 'translateX(2000px)';

      anime({
        targets: this.chart,
        translateY: [
          { value: 1000, duration: 1000 }
        ],
        easing: [.91,-0.54,.29,1.56],
        backgroundColor: '#FFF',
        duration: 1000,
        loop: false
      });

      anime({
        targets: this.button,
        opacity: 1,
        easing: 'linear',
        duration: 500
      });

      anime({
        targets: this.resultContainer,
        translateX: [
          { value: 2000, duration: 500 },
          { value: 0, duration: 500 }
        ],
        easing: [.91,-0.54,.29,1.56],
        backgroundColor: '#FFF',
        duration: 1000,
        loop: false
      });

    },
    spinAgain: function ()  {

      this.prize.style.zIndex = 0;

      anime({
        targets: this.chart,
        translateY: [
          { value: 0, duration: 1000 }
        ],
        easing: [.91,-0.54,.29,1.56],
        backgroundColor: '#FFF',
        duration: 1000,
        loop: false
      });

      anime({
        targets: this.button,
        opacity: 0,
        easing: 'linear',
        duration: 100
      });

      anime({
        targets: this.resultContainer,
        translateX: [
          { value: 0, duration: 250 },
          { value: 1000, duration: 250 }
        ],
        easing: [.91,-0.54,.29,1.56],
        backgroundColor: '#FFF',
        duration: 500,
        loop: false
      });
    },
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

svg {
  padding: 50px;
  margin-left: 35px;
}

#chart {
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
  padding-top: 40px;
}
#logo {
  position: absolute;
  width: 130px;
  padding-top: 10px;
  border-radius: 50%;
}
#logo:hover {
  cursor: pointer;
}
#prize {
  padding: 0 50px;
  position: absolute;
  margin: 0 auto;
  width: 90%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
.button {
  background-color: #35495E;
  color: #fff;
  border: none;
  border-radius: 2px;
  margin-top: 20px;
  width: 200px;
  height: 70px;
  font-size: 1.5rem;
  text-transform: uppercase;
  transition: 1s;
}
#button {
  opacity: 0;
}
.button:hover {
  background-color: #b01538;
  cursor: pointer;
  opacity: 0;
}


/*MEDIA QUERIES*/
@media screen and (max-width: 500px) {
  #chart {
    margin-left: -500px;
  }
  #logo {
    right: 30px;
  }
}
@media screen and (max-width: 360px) {
  #logo {
    right: 30px;
    top: 50px;
  }
}
</style>
