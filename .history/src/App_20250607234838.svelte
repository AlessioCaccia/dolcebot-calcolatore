<script>
  let fatturato = 0;
  let costoPulizia = 0;
  let checkIn = 0;
  let aliquota = 0.21;

  // Derived values
  $: ota = fatturato * 0.18;
  $: pulizie = costoPulizia * checkIn;

  $: auto_ced = fatturato * aliquota;
  $: auto_netto = fatturato - ota - pulizie - auto_ced;

  $: pm_comm = (fatturato - ota - pulizie) * 0.3;
  $: pm_ced = (fatturato - ota - pulizie - pm_comm) * aliquota;
  $: pm_netto = fatturato - ota - pulizie - pm_comm - pm_ced;

  $: db_comm = (fatturato - ota - pulizie) * 0.06;
  $: db_ced = (fatturato - ota - pulizie - db_comm) * aliquota;
  $: db_netto = fatturato - ota - pulizie - db_comm - db_ced;
</script>

<div class="input-grid">
  <div class="input-group">
    <label for="fatturato">Fatturato lordo immobile (€)</label>
    <input
      type="number"
      id="fatturato"
      bind:value={fatturato}
      placeholder="Es. 30000"
    />
  </div>

  <div class="input-group">
    <label for="costoPulizia">Costo singola pulizia (€)</label>
    <input
      type="number"
      id="costoPulizia"
      bind:value={costoPulizia}
      placeholder="Es. 50"
    />
  </div>

  <div class="input-group">
    <label for="checkIn">Numero check-in annuali</label>
    <input
      type="number"
      id="checkIn"
      bind:value={checkIn}
      placeholder="Es. 40"
    />
  </div>

  <div class="input-group">
    <label for="aliquota">Cedolare secca (%)</label>
    <select id="aliquota" bind:value={aliquota}>
      <option value="0.21">21%</option>
      <option value="0.26">26%</option>
    </select>
  </div>
</div>

<div class="main-container">
  <div class="grid-container">
    <div class="grid-header-cell other-header border-0"></div>
    <div class="grid-header-cell">
      <img
        src="https://dolcebot.komunikasi.it/wp-content/uploads/2025/06/logo_scuro.svg"
        alt="DolceBot Logo"
        class="dolcebot-logo"
      />
    </div>
    <div class="grid-header-cell other-header">GESTIONE AUTONOMA</div>
    <div class="grid-header-cell other-header">GESTIONE PM</div>

    <div class="grid-data-cell">Commissioni OTA</div>
    <div class="grid-data-cell">{ota.toFixed(2)}</div>
    <div class="grid-data-cell">{ota.toFixed(2)}</div>
    <div class="grid-data-cell">{ota.toFixed(2)}</div>

    <div class="grid-data-cell">Pulizie</div>
    <div class="grid-data-cell">{pulizie.toFixed(2)}</div>
    <div class="grid-data-cell">{pulizie.toFixed(2)}</div>
    <div class="grid-data-cell">{pulizie.toFixed(2)}</div>

    <div class="grid-data-cell">Cedolare secca</div>
    <div class="grid-data-cell">{db_ced.toFixed(2)}</div>
    <div class="grid-data-cell">{auto_ced.toFixed(2)}</div>
    <div class="grid-data-cell">{pm_ced.toFixed(2)}</div>

    <div class="grid-data-cell">Commissioni gestore</div>
    <div class="grid-data-cell">{db_comm.toFixed(2)}</div>
    <div class="grid-data-cell">0.00</div>
    <div class="grid-data-cell">{pm_comm.toFixed(2)}</div>

    <div class="grid-data-cell net-profit-row-item">Netto proprietario</div>
    <div class="grid-data-cell net-profit-row-item">{db_netto.toFixed(2)}</div>
    <div class="grid-data-cell net-profit-row-item">
      {auto_netto.toFixed(2)}
    </div>
    <div class="grid-data-cell net-profit-row-item">{pm_netto.toFixed(2)}</div>
  </div>
</div>

<style>
  body {
    font-family: "Raleway", sans-serif;
  }

  label {
    font-family: "Raleway", sans-serif;

    display: block;
    margin: 1rem 0 0.5rem;
  }

  input,
  select {
    font-family: "Raleway", sans-serif;

    width: 100%;
    padding: 0.75rem;
    font-size: 20px;
    box-sizing: border-box;
  }

  .main-container {
    font-family: "Raleway", sans-serif;

    overflow-x: auto;
    margin-top: 2rem;
    background: white;
    border-radius: 10px 10px 0 0;
    border: 1px solid #e5e5e5;
  }

  .grid-container {
    min-width: 768px;
    display: grid;
    grid-template-columns: 1.5fr 1fr 1fr 1fr;
    border-collapse: collapse;
  }

  .grid-header-cell,
  .grid-data-cell {
    padding: 13px;
    text-align: center;
    border: 1px solid #e5e5e5;
    background-color: white;
    color: #162047;
    align-content: center;
  }

  .other-header {
    font-weight: bold;
    font-size: 20px;
  }

  .grid-data-cell:nth-child(4n + 1) {
    text-align: left;
    font-weight: bold;
  }

  .net-profit-row-item {
    background-color: #0057ff !important;
    color: white;
    font-weight: 600;
    border: 0;
  }

  img.dolcebot-logo {
    max-width: 100%;
    height: auto;
  }

  .input-grid {
    display: flex;
    flex-wrap: wrap;
    gap: 1rem;
    margin-bottom: 2rem;
  }

  .input-group {
    flex: 1 1 calc(50% - 1rem);
    display: flex;
    flex-direction: column;
  }

  @media (max-width: 600px) {
    .input-group {
      flex: 1 1 100%;
    }
  }

  .input-group label {
    font-size: 0.85rem;
    margin-bottom: 0.25rem;
    color: #333;
  }

  .input-group input,
  .input-group select {
    width: 100%;
    padding: 0.6rem 0.75rem;
    font-size: 20px;
    border: 1px solid #ccc;
    border-radius: 10px;
  }
</style>
