<script>
    import { onMount } from 'svelte';
    import * as d3 from 'd3';
    
    import {provData} from '../provData.js';
    
    
    onMount(() => {
    
       /* Elementos DOM */
    
       const lv_nomLloc = document.getElementById('lv_nomLloc');
       const lv_pobl = document.getElementById('lv_pobl');
       const lv_poblMedida = document.getElementById('lv_poblMedida');
       const lv_dataT = document.getElementById('lv_dataT');
       const lv_barContent = document.getElementById('lv_barContent');
       const lv_barLloc = document.getElementById('lv_barLloc');
       const lv_dataN = document.getElementById('lv_dataN');
       const lv_dataNgroup = document.querySelector(".lv_dataNgroup");
       const lv_indMin = document.getElementById('lv_indMin');
       const lv_indMax = document.getElementById('lv_indMax');
    
    
    
    
    
    
    
    
    
    
    
       /* Sugerencias INPUT */
    
       const lv_inputData = document.getElementById('lv_inputData');
       const lv_suggestions = document.getElementById('lv_suggestions');
    
    
       function displaySuggestions(inputValue) {
               lv_suggestions.style.display = "block";
               let p = document.createElement("p"); 
               lv_suggestions.innerHTML = '';
               if (inputValue.length >= 1) {
                   const filteredData = provData.filter(item => item.provincia.toLowerCase().startsWith(inputValue.toLowerCase()));
                   filteredData.forEach(item => {
                       const suggestionDiv = document.createElement('div');
                       let p = document.createElement('p');
                       p.textContent = item.provincia;
                       suggestionDiv.classList.add("lv_suggItem");
                       suggestionDiv.addEventListener('click', () => {
                           lv_inputData.value = item.provincia;
                           lv_suggestions.innerHTML = '';
                           console.log(lv_inputData.value);
                           let inputData = lv_inputData.value.toLowerCase();
                           const resultado = provData.find(item => item.provincia.toLowerCase() === inputData);
    
                           lv_pintaGraph(resultado);
                           
                       });
                       suggestionDiv.appendChild(p)
                       lv_suggestions.appendChild(suggestionDiv);
                   });
               }
       }
    
       lv_inputData.addEventListener('input', () => {
           displaySuggestions(lv_inputData.value);
       });
    
       window.addEventListener('click', function(event) {
           if (!lv_inputData.contains(event.target) && !lv_suggestions.contains(event.target)) {
               lv_suggestions.style.display = "none";
           }
       });
    
    
    
    
    
    
    
    
    
    
    
    
       
    
    
    
    
    
       /* GRÁFICA 1 */
       
    
       function lv_pintaGraph(res){
    
           lv_barContent.style.width = "0";
    
    
           let range = 650;
           let lv_tasaIncrem = (res.dias_aire_ara / res.dias_aire_abans).toFixed(1).toString();
    
           lv_nomLloc.textContent = res.provincia;
          
           
           if (res.pobl >= 1000000) {
               const millones = res.pobl / 1000000;         
               
               lv_pobl.textContent = millones % 1 !== 0 ? millones.toFixed(1).replace('.', ',') : Math.floor(millones).toString();
           } else {
               lv_pobl.textContent = res.pobl.toLocaleString('es-ES');
           }
    
           
    
           if(res.pobl < 1000000){
               lv_poblMedida.style.display = "none";
    
           }else{
               lv_poblMedida.style.display = "block";
               /* recortar a  */
           }
    
    
    
    
           if(res.aire_impact === "high"){
               lv_dataN.textContent = "alto";
               lv_dataNgroup.style.backgroundColor = "#d9535a";
               lv_dataNgroup.classList.add("lv_lightMode");
    
           }else if(res.aire_impact === "low"){
               lv_dataN.textContent = "medio";
               lv_dataNgroup.classList.add("lv_lightMode");
               lv_dataNgroup.style.backgroundColor = "#eb7f84";
    
           }else{
               lv_dataN.textContent = "leve";
               lv_dataNgroup.classList.remove("lv_lightMode");
               lv_dataNgroup.style.backgroundColor = "#f6d6d3";
           } 
    
    
           lv_dataT.textContent = lv_tasaIncrem;
    
    
           /* Rell barras hor */
    
           let widthBar = Math.abs(((res.dias_aire_ara - res.dias_aire_abans) / range) * 100);
           let posStart = (res.dias_aire_abans / range) * 100;
           let posEnd = (res.dias_aire_ara / range) * 100;
    
           lv_barContent.style.width = widthBar + '%';
           lv_barContent.style.left = posStart + '%';
    
    
    
    
    
           lv_barLloc.textContent = res.provincia;
    
           lv_indMin.style.left = `${posStart -4}%`;
           lv_indMin.textContent =  `${Math.trunc(res.dias_aire_abans)}`;
    
           lv_indMax.style.left = posEnd + '%';
           lv_indMax.textContent = `${Math.trunc(res.dias_aire_ara)}`;
    
    
           lv_createBarChart(res.evolAire);
    
       }
    
    
    
    
    
       lv_pintaGraph(provData[21]);
       
    
    });
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    /* GRÁFICO DE BARRAS */
    
    
    
    
    
    const lv_years = Array.from({ length: 45 }, (v, i) => 1979 + i);
    
    
    
    
    function lv_createBarChart(lv_data) {
       const svgWidth = 800;
       const svgHeight = 400;
       const margin = { top: 20, right: 30, bottom: 40, left: 50 };
    
       
       d3.select('svg').remove();
    
       const svg = d3.select('#lv_barChart')
         .append('svg')
         .attr('viewBox', `0 0 ${svgWidth} ${svgHeight}`) 
         .attr('preserveAspectRatio', 'xMinYMin meet') 
         .style('width', '100%')  
         .style('height', '100%');  
    
    
       const chartGroup = svg.append('g')
         .attr('transform', `translate(${margin.left},${margin.top})`);
    
    
       const x = d3.scaleBand()
         .domain(lv_years)
         .range([0, svgWidth - margin.left - margin.right])
         .padding(0.1);
    
       const y = d3.scaleLinear()
         .domain(d3.extent(lv_data)) 
         .nice()
         .range([svgHeight - margin.top - margin.bottom, 0]);
    
    
    
       
       chartGroup.append('g')
         .attr('class', 'lv_hTicks')
         .attr('transform', `translate(0,${svgHeight - margin.top - margin.bottom})`)
         .call(d3.axisBottom(x).tickValues([1980, 1990, 2000, 2010, 2020]))
         .style("font-size","1rem")
         .style("font-family", "Roboto Condensed")
         .style("color", "#BBBBBB");
    
    
      
         const yAxis = chartGroup.append('g')
         .call(d3.axisLeft(y).ticks(3))
         .style("font-size","1rem")
         .attr("class", "lv_vert")
         .style("color", "#BBBBBB")
         .style("font-family", "Roboto Condensed");
    
         /* yAxis.select(":last-child text")
           .text(function(d) {
               return d + " ºC";
           }); */
    
         
       
       chartGroup.append("g")
         .call(d3.axisLeft(y)
         .ticks(3)
         .tickSize(-svgWidth + margin.left + margin.right)
         .tickFormat('')) 
         .attr("class", "lv_grid");
         
    
      
       /* chartGroup.selectAll('.lv_bar')
         .data(lv_data)
         .enter()
         .append('rect')
         .attr('class', 'lv_bar')
         .attr('x', (d, i) => x(lv_years[i]))
         .attr('y', d => y(d))
         .attr('width', x.bandwidth())
         .attr('height', d => svgHeight - margin.top - margin.bottom - y(d))
         .attr('fill', '#288AE4'); */
    
    
         chartGroup.selectAll(".lv_bar")
         .data(lv_data)
         .enter()
         .append("rect")
         .attr("class", "lv_bar")
         .attr('x', (d, i) => x(lv_years[i]))
         .attr("width", x.bandwidth())
         .attr("y", svgHeight - margin.top - margin.bottom)  
         .attr("height", 0)  
         .transition()  
         .duration(1000) 
         .attr("y", d => y(d)) 
         .attr("height", d => svgHeight - margin.top - margin.bottom - y(d))
         .attr('fill', '#d9535a'); 
         
         
         
         
     }
    
    
    
    
     
    
    
    
    
    
    
    
    </script>
    
    
    
    
    
    
    
    
    
    
    
    <div id="lv_widget">
       <div class="lv_widgetCont">
    
           <div class="lv_widgetWrapper">
    
               <div id="lv_Title">
                   <div class="lv_titleWrapper">
                       <h2>¿Cuánto ha aumentado la demanda de aire acondicionado en tu provincia respecto a 1980?</h2>
                   </div>
               </div>
    
    
    
    
    
    
               <div id="lv_Busc">
                   <div class="lv_buscWrapper">
                       <div class="lv_buscInput">
                           <input type="text" id="lv_inputData" placeholder="Busca tu provincia">
                           <div id="lv_suggestions" class="lv_suggestions"></div>
                       </div>
                           
    
                   </div>
               </div>
    
    
    
    
    
    
               <div id="lv_Graph">
    
                   <div class="lv_Lloc">
                       <div class="lv_llocWrapper">
                           <h3 id="lv_nomLloc"></h3>
                           <p> <span id="lv_pobl"></span> <span id="lv_poblMedida">millones de</span> habitantes</p>
                       </div>
    
                   </div>
    
    
    
                   
    
    
    
    
                   <div id="lv_Detall1" class="lv_Detall">
                       <p>La demanda de aire acondicionado se ha multiplicado por 
                           <span id="lv_dataT"></span> 
                           en comparación con la década de 1980</p>
                   </div>
    
                   <div id="lv_Detall2" class="lv_Detall">
    
                       <p>El valor de enfriamiento calcula los grados acumulados y sirve para estimar el impacto que las altas temperaturas tienen sobre la demanda eléctrica de un país
 
 
                       </p>
                   </div>
    
    
    
    
                   <div class="lv_Graphic">
    
                       <div class="lv_graphLey">
                           <p>1978 - 1988</p>
                           <p>2013 - 2023</p>
                       </div>
    
    
                       <div id="lv_barProv" class="lv_bar lv_barProv">
    
    
                           <div class="lv_barLloc">
                               <p id="lv_barLloc"></p>
                           </div>
    
                           <div class="lv_barGraph">
                               <div class="lv_bar_range">
                                   <div id="lv_barContent" class="lv_barContent"></div>
                               </div>
    
    
                               <div class="lv_indicadores">
                                   <span id="lv_indMin"></span>
                                   <span id="lv_indMax"></span>
                               </div>
    
    
                           </div>
    
    
                
                       </div>
    
    
    
    
    
    
    
                        <div id="lv_barEsp" class="lv_bar lv_barEsp">
    
    
                           <div class="lv_barLloc">
                               <p id="lv_barLloc">España</p>
                           </div>
    
                           <div class="lv_barGraph">
                               <div class="lv_bar_range">
                                   <div id="lv_barContent" class="lv_barContent" style="width: 25%; left: 19.9186%;"></div>
                               </div>
    
    
                               <div class="lv_indicadores">
                                   <span style="left: 15%;">153</span>
                                   <span style="left: 45%;">259</span>
                               </div>
    
    
                           </div>
    
    
    
    
    
    
    
    
    
                               
                       </div>
    
                   </div>
    
    
    
    
    
    
    
                  
    
                   
               </div>
    
    
    
               <div class="lv_Tag">
                   <p> <span class="lv_dataNgroup">Incremento <span id="lv_dataN"></span></span> </p>
               </div>
    
    
              
    
    
    
    
    
    
               <div id="lv_Bars">
                   <div class="lv_barsWrapper">
    
                       <div class="lv_barsTitle">
                           <p>Valor de enfriamiento</p>
                       </div>
    
    
                       <div class="lv_barsGraph">
                           <div class="lv_barsGraphWrapper">
    
                               <div id="lv_barChart"> </div>
                               
                           </div>
    
                       </div>
    
    
    
                   </div>
               </div>
    
    
    
    
    
           </div>
           
    
    
    
    
       </div>
    </div>
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    <style>
    
       :root{
           --lv_blue: #001c4c;
           --lv_graphGrey: #BBBBBB;
           --lv_graphRed: rgb(244, 113, 113);
           --lv_graphBlue: #d9535a;
       }
    
       
         
    
       #lv_widget{
           font-family: 'Roboto Condensed', sans-serif;
           margin-bottom: 50px;
    
       }
    
       .lv_widgetCont{
           width: 100%;
           max-width: 750px;
           margin:0 auto;
           
           
       }
    
       .lv_widgetWrapper{
           display: flex;
           flex-direction: column;
           justify-content: flex-start;
           align-items: center;
    
    
       }
    
    
    
       /* TITLE */
    
       #lv_Title{
           width: 100%;
           max-width: 750px;
           margin: 0 auto 20px auto;
    
       }
    
       .lv_titleWrapper{
           width: 100%;
           padding-left: 10%;
           padding-right: 10%;
           margin: 0 auto;
           
       }
    
       .lv_titleWrapper h2{
           text-align: center;
           font-weight: 400;
           font-size:1.3rem;
       }
    
    
    
    
    
    
    
    
    
    
       /* BUSCADOR */
    
    
       #lv_Busc{
           width:100%;
           margin-bottom: 20px;
       }
    
       .lv_buscWrapper{
           width: 100%;
           display: flex;
           justify-content:center;
           column-gap: 40px;
    
       }
    
       .lv_buscInput{
           
           height: 33px;
           position:relative;
    
       }
    
    
       .lv_buscInput input{
           width: 170px;
           height: 43px;
           border-radius: 1px;
           border-color: #CBC8C8;
           outline: none;
           -webkit-appearance: none;
           -moz-appearance: none;
           appearance: none;
           border-style:solid;
           font-family: 'Roboto Condensed', sans-serif;
           font-size: 1.1rem;
           
       }
    
       ::placeholder {
           color: #CBC8C8;
           font-family: 'Roboto Condensed', sans-serif;
           font-size: 1.1rem;
           padding-left: 7px;
           
       }
    
       .lv_suggestions{
           position: absolute;
           top: 44px;
           left:0;
           width: 100%;
           background-color: rgb(240, 239, 239);
           cursor: pointer;
           z-index: 4;
       }
    
      
    
        :global(.lv_suggItem p){
           margin: 0;
           padding: 7px;
           margin-left: 6px;
           margin-right: 6px;
           border-bottom: 1px solid #e0e0e0;
    
       }
    
    
       :global(.lv_suggItem p:hover){
           background-color: #d0d0d0;
    
       }
    
    
    
       #lv_Busc button{
           width: 70px;
           height: 43px;
           color: #fff;
           background-color:var(--lv_blue);
           font-family: 'Roboto Condensed', sans-serif;
           border-color: #CBC8C8;
           font-size: 1.1rem;
           cursor:pointer;
    
       }
    
    
    
    
    
    
    
    
    
       /* GRAPH 1 */
    
       #lv_Graph{
           display: flex;
           flex-direction: column;
           justify-content: center;
           align-items: center;
       }
    
    
    
       .lv_Lloc{
    
       }
    
    
       .lv_llocWrapper{
           
       }
    
       .lv_Lloc h3{
           text-align: center;
           font-weight: 700;
           font-size: 1.4rem;
           margin-bottom:6px;
       }
    
    
    
    
       .lv_Lloc p{
           text-align: center;
           margin-top: 0;
           display: flex;
       }
    
    
       .lv_Lloc p span{
           margin-right: 4px;
       }
    
    
    
    
    
    
    
    
       .lv_Detall{
    
       }
    
       .lv_Detall p{
           text-align: center;
           padding-left: 10%;
           padding-right: 10%;
           font-size: 1.1rem;
    
       }
    
    
       #lv_dataT{
           padding: 2px 4px;
           background-color: var(--lv_graphBlue);
           color: #fff;
       }
    
    
    
    
    
    
    
    
       .lv_Graphic{
           width: 100%;
           padding-right: 4%;
           padding-left: 2%;
 
 
       }
    
       .lv_graphLey{
           display: flex;
           justify-content: space-evenly;
       }
    
    
       .lv_graphLey p{
           color: var(--lv_graphGrey);
           font-size: 1.1rem;
       }
    
    
    
    
    
       /* Comparativa */
    
    
       .lv_bar{
           width: 100%;
           display: grid;
           grid-template-columns: repeat(10, 1fr);
       }
    
       #lv_barProv{
           margin-bottom:20px;
    
       }
    
    
       #lv_barEsp{
           margin-bottom:40px;
       }
    
    
       .lv_barLloc, .lv_barGraph, .lv_barData{
           align-self: center;
    
       }
    
       .lv_bar p{
           margin: 0;
       }
    
    
       .lv_barLloc{
           grid-column: 1 / span 2;
    
       }
    
       .lv_barLloc p{
           margin-right: 7px;
           color: var(--lv_graphGrey);
           text-align: left;
           
       }
    
    
    
    
    
       .lv_barGraph{
           grid-column: 3 / span 8;
           position: relative;
       }
    
    
       .lv_bar_range{
           /* margin-top: 15px; */
           background-color: var(--lv_graphGrey);
           width: 100%;
           height: 8px;
           position: relative;
    
       }
    
    
    
    
    
       
    
       .lv_barContent{
           width: 0; 
           transition: width 1s ease;
           height: 100%;
           position: absolute;
           top: 0;
           background-color:var(--lv_graphBlue);
           clip-path: polygon(0 0, 90% 0, 100% 50%, 90% 100%, 0 100%);
           
           
       }
    
       /* .lv_barContent:before{
           content: "";
           width: 13px;
           height: 15px;
           background-color:var(--lv_graphBlue);
           position: absolute;
           top: 50%;
           left: 0px; 
           transform: translateY(-50%);
       }
    
    
    
       .lv_barContent:after{
           content: "";
           width: 0;
           height: 0;
           border-top: 10px solid transparent;
           border-bottom: 10px solid transparent;
           border-left: 15px solid var(--lv_graphBlue); 
           position: absolute;
           top: 50%;
           right: -10px; 
           transform: translateY(-50%);
       } */
    
    
    
    
    
       .lv_indicadores{
           position: relative;
           top: 5px;
           width: 100%;
           color:var(--lv_graphBlue);
           font-weight: bold;
       }
    
    
       .lv_indicadores span{
           position: absolute;
       }
    
    
      
    
    
    
    
    
       .lv_barData p{
           color:var(--lv_graphBlue);
           font-size: 1rem;
           font-weight: bold;
           font-size: 1.1rem;
           
       }
    
    
    
    
    
      
    
       .lv_dataNgroup{
           padding: 4px 12px;
       }
    
       :global(.lv_dataNgroup.lv_lightMode){
           color: #fff;
       }
    
    
    
    
    /*    #lv_dataI, #lv_dataF{
           border: 1px dashed var(--lv_graphBlue);
           padding: 1px 2px;
       } */
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
       /* GRAPH 2 */
    
       #lv_Bars{
           width: 100%;
    
       }
    
       .lv_barsWrapper{
           width: 100%;
    
    
       }
    
    
       .lv_barsTitle{
        padding-left: 2%;
    
       }
    
    
       .lv_barsTitle p{
           color:var(--lv_graphGrey);
           margin-bottom: 0;
           
       }
    
    
    
    
    
       .lv_barsGraph{
    
       }
    
    
       .lv_barsGraphWrapper{
           
    
       }
    
    
       #lv_barChart{
           width: 100%;
           height: 100%;
           position: relative;
       }
    
    
       #lv_barChart svg{
    
       }
    
       .lv_bar {
           fill: var(--lv_graphBlue)
       }
    
    
    
    
    
       /* TICKS / GRID */
    
    
       text{
           font-size: 2rem !important;
       }
    
    
       :global(.lv_grid .tick line){
           stroke: lightgrey;
           stroke-opacity: 0.7;
       }
    
       :global(.lv_vert path.domain){
           display: none !important;
       }
    
       :global(.lv_vert line){
           display: none !important;
       }
    
       :global(.lv_grid path.domain){
           display: none !important;
       }
    
    
       
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
       
    </style>