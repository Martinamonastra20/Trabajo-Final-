<script>

  import { polygonArea } from "d3";


  import { sportsData, muscles } from './data.js';


  let sliderValue = 0; // El valor del slider, desde 0 hasta 100

  // Función para calcular la extensión de la línea superior
  const calculateWidth = (value) => {
    return (value / 100) * 150; // Esto hace que la línea superior crezca hasta 150px
  };

// Variable para el deporte seleccionado y resaltar
let selectedSport = "Futbol";
  let resaltar = "Todos";

  // Datos reactivos para las partes del cuerpo destacadas y resaltadas
  let highlightedBodyParts = [];
  let resaltarBodyParts = [];

  // Actualiza las zonas destacadas según el deporte
  $: highlightedBodyParts = sportsData.find((s) => s.sport === selectedSport)?.muscles || [];

  // Actualiza las zonas resaltadas
  $: {
    resaltarBodyParts =
      resaltar === "Todos"
        ? highlightedBodyParts
        : highlightedBodyParts.filter((muscle) => muscle === resaltar);
  }

  // Tooltip para mostrar información
  let tooltipVisible = false;
  let tooltipX = 0, tooltipY = 0;
  let tooltipContent = "";

  function showTooltip(e, bodyPart) {
    tooltipVisible = true;
    tooltipContent = `Zona: ${bodyPart}`;
    tooltipX = e.pageX;
    tooltipY = e.pageY;
  }

  function hideTooltip() {
    tooltipVisible = false;
  }
</script>

<h2>cuerpos</h2>

<!-- Selección del deporte -->
<label for="sport">Deporte:</label>
<select id="sport" bind:value={selectedSport}>
  {#each sportsData as { sport }}
    <option value={sport}>{sport}</option>
  {/each}
</select>

<!-- Selección de resaltar zonas musculares -->
<h3>Resaltar</h3>
<label for="resaltar">Resaltar:</label>
<select id="resaltar" bind:value={resaltar}>
  <option value="Todos">Todos</option>
  {#each highlightedBodyParts as muscle}
    <option value={muscle}>{muscle}</option>
  {/each}
</select>

<div class="heatmap">

  <!-- Brazos -->
  <img
   src="/images/Brazo.svg" alt= "Brazos y hombros " 
   
    class="body-part 
      {highlightedBodyParts.includes('Brazos y hombros') ? 'highlight' : ''} 
      {resaltarBodyParts.includes('Brazos y hombros') ? 'resaltar' : ''}"
    style="top: 100px;"
    on:mouseenter={(e) => showTooltip(e, 'Brazos y hombros')}
    on:mouseleave={hideTooltip}
  />

  <!-- Torzo -->
  <img
     src="/images/Torzo.svg" alt= "Torzo" 
   
    class="body-part 
      {highlightedBodyParts.includes('Torzo') ? 'highlight' : ''} 
      {resaltarBodyParts.includes('Torzo') ? 'resaltar' : ''}"
    style="top: 200px;"
    on:mouseenter={(e) => showTooltip(e, 'Torzo')}
    on:mouseleave={hideTooltip}
  />

  <!-- Piernas -->
  <img
    src="/images/Piernas.svg" alt= "Piernas"
    
    class="body-part 
      {highlightedBodyParts.includes('Piernas') ? 'highlight' : ''} 
      {resaltarBodyParts.includes('Piernas') ? 'resaltar' : ''}"
    style="top: 300px;"
    on:mouseenter={(e) => showTooltip(e, 'Piernas')}
    on:mouseleave={hideTooltip}
  />
</div>

{#if tooltipVisible}
<div class="tooltip" style="left: {tooltipX}px; top: {tooltipY}px">
  {tooltipContent}
</div>
{/if}

<main>
  <div class="header">
    <div class="nombres">
      <p>Candelaria Victorica</p>
      <p>Sofía Rolón</p>
      <p>Martina Monastra</p>
    </div>

    <h3 class="headline">
      <b>El Deporte Como Escultura Corporal</b>
    </h3>
    <p class="bajada">
      El deporta ha moldeado los cuerpos de las personas desde hace muchos años.
      Acompáñanos a ver los cambios corporales según los deportes de alto
      rendimiento.
    </p>
  </div>

  <div class="coffes-container">
    <div class="column-wrapper">
      <div class="persona">
        <div id="torso">
          <img
            src="./images/Torzo.svg"
            alt="persona"
            style="width: 65%;"
          />
        </div>
        <div id="brazo">
          <img src="./images/Brazo.svg" alt="brazo" />
        </div>
        <div id="pierna">
          <img src="./images/Piernas.svg" alt="Piernas" />
        </div>
      </div>
    </div>
  </div>

  <div class="container">
    <!-- Rectángulo deformable -->
    <div
  style="clip-path: polygon(
    50% 0%, 
    calc(50% - {calculateWidth(sliderValue) / 2}px) 100%, 
    calc(50% + {calculateWidth(sliderValue) / 2}px) 100%
  );"
></div>

 

  <!-- Slider para controlar la deformación -->
  <input
    type="range"
    min="35"
    max="100"
    bind:value={sliderValue}
    class="slider"
  />
 
</main>
  

<style>
  /* Estilos del contenedor del rectángulo */
  .container {
    width: 300px;
    height: 400px;
    margin: 50px auto;
    position: relative;
  }

  /* Estilos del rectángulo */
  .rectangle {
  position: absolute;
  top: 50px; /* Desplaza el rectángulo hacia abajo */
  left: 0;
  width: 100%;
  height: 100%;
  background-color: #3498db;
  transition: clip-path 0.3s ease, transform 0.3s ease;
  transform: rotate(180deg); /* Aquí aplicas la rotación */
}

  /* Slider */
  .slider {
    width: 100%;
    margin-top: 20px;
  }

  #torso {
    position: absolute;
    margin-left: 40px;
  }
  #brazo {
    position: absolute;
    margin-top: 80px;
  }

  #pierna {
    position: absolute;
    margin-top: 220px;
  }

  .coffes-container {
    display: flex;
    gap: 50px;
  }
  .column-wrapper {
    position: relative;
    width: 160px;
    height: 200px;
    /* background-color: #ccc;
  border: black 1px solid; */
  }

  .nombres {
    text-decoration: none;
    display: flex;
    gap: 20px;
  }
  .header {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    /* margin-top: 20px;
  margin-bottom: 80px; */
  }
  .headline {
    font-size: 40px;
    font-weight: 300;
    line-height: 1.2;
    text-align: center;
    margin: 20px;
  }
  .bajada {
    font-size: 24px;
    font-weight: 300;
    text-align: center;
    margin: 10px;
  }
  .headline b {
    display: block;
  }
  .heatmap {
    position: relative;
    width: 300px;
    height: 600px;
    margin: auto;
  }

  .body-part {
    position: absolute;
    width: 100%;
    height: auto;
    opacity: 0.2;
    transition: opacity 0.3s, transform 0.3s;
  }

  .highlight {
    opacity: 1;
    transform: scale(1.1);
  }

  .resaltar {
    border: 2px solid yellow;
  }

  .tooltip {
    position: absolute;
    background-color: rgba(0, 0, 0, 0.7);
    color: white;
    padding: 5px;
    border-radius: 3px;
    pointer-events: none;
  }
</style>