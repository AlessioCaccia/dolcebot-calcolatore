<script>
  let fatturato = 0;
  let costoPulizia = 0;
  let checkIn = 0;
  let aliquota = 0.21;
  let aliquota_options = [
    {
      label: "21%",
      value: 0.21,
    },
    {
      label: "26%",
      value: 0.26,
    },
  ];

  const formatter = new Intl.NumberFormat("it-IT", {
    maximumFractionDigits: 2,
  });

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

  // svelte .detail onclick
  // @ts-ignore
  function toggleDetail(event) {
    const target = event.target;

    const targetClass = target.classList[0];

    const nextSiblings = document.querySelectorAll(`.tred.${targetClass}`);

    if (target.classList.contains("toggled")) {
      // @ts-ignore
      target.classList.remove("toggled");
    } else {
      // @ts-ignore
      target.classList.add("toggled");
    }

    nextSiblings.forEach((sibling) => {
      // @ts-ignore
      if (sibling.classList.contains("collapsed")) {
        // @ts-ignore
        sibling.classList.remove("collapsed");
      } else {
        // @ts-ignore
        sibling.classList.add("collapsed");
      }
    });
  }
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
      {#each aliquota_options as option}
        <option value={option.value}>
          {option.label || option.text}
        </option>
      {/each}
    </select>
  </div>
</div>
<div class="main-container">
  <div class="grid-container">
    <div class="grid-header-cell other-header b0 dnone-mobile"></div>
    <div class="grid-header-cell other-header bt-0">
      <picture>
        <source
          srcset="https://www.dolcebot.com/wp-content/uploads/2025/06/Group-1.svg"
          media="(max-width: 768px)"
        />
        <img
          src="https://www.dolcebot.com/wp-content/uploads/2025/06/logo_scuro.svg"
          alt="DolceBot Logo"
          class="dolcebot-logo-img"
        />
      </picture>
    </div>
    <div class="grid-header-cell other-header bt-0">GESTIONE AUTONOMA</div>
    <div class="grid-header-cell other-header bt-0">GESTIONE PM</div>

    <!-- Fatturato netto mobile -->

    <div class="grid-data-cell net-profit-row-item detail dnone-desktop">
      Netto proprietario
    </div>
    <div class="grid-data-cell net-profit-row-item dnone-desktop">
      € {formatter.format(db_netto)}
    </div>
    <div class="grid-data-cell net-profit-row-item dnone-desktop">
      € {formatter.format(auto_netto)}
    </div>
    <div class="grid-data-cell net-profit-row-item dnone-desktop">
      € {formatter.format(pm_netto)}
    </div>

    <!-- end fatturato netto mobile -->

    <button
      type="button"
      aria-label="Toggle"
      class="ota grid-data-cell detail collapse-cta toggled"
      on:click={toggleDetail}
    >
      Commissioni OTA <i class="dnone-desktop">🡩</i>
    </button>

    <div class="grid-data-cell tred ota">€ {formatter.format(ota)}</div>
    <div class="grid-data-cell tred ota">€ {formatter.format(ota)}</div>
    <div class="grid-data-cell tred ota">€ {formatter.format(ota)}</div>

    <button
      type="button"
      aria-label="Toggle"
      class="pulizie grid-data-cell detail collapse-cta"
      on:click={toggleDetail}
    >
      Pulizie <i class="dnone-desktop">🡩</i>
    </button>

    <div class="grid-data-cell tred pulizie collapsed">
      € {formatter.format(pulizie)}
    </div>
    <div class="grid-data-cell tred pulizie collapsed">
      € {formatter.format(pulizie)}
    </div>
    <div class="grid-data-cell tred pulizie collapsed">
      € {formatter.format(pulizie)}
    </div>

    <button
      type="button"
      aria-label="Toggle"
      class="cedolare grid-data-cell detail collapse-cta"
      on:click={toggleDetail}
    >
      Cedolare secca <i class="dnone-desktop">🡩</i>
    </button>

    <div class="grid-data-cell tred cedolare collapsed">
      € {formatter.format(db_ced)}
    </div>
    <div class="grid-data-cell tred cedolare collapsed">
      € {formatter.format(auto_ced)}
    </div>
    <div class="grid-data-cell tred cedolare collapsed">
      € {formatter.format(pm_ced)}
    </div>

    <button
      type="button"
      aria-label="Toggle"
      class="comm-gest grid-data-cell detail collapse-cta"
      on:click={toggleDetail}
    >
      Commissioni gestore <i class="dnone-desktop">🡩</i>
    </button>

    <div class="grid-data-cell tred comm-gest collapsed">
      €{formatter.format(db_comm)}
    </div>
    <div class="grid-data-cell tred comm-gest collapsed">€ 0</div>
    <div class="grid-data-cell tred comm-gest collapsed">
      € {formatter.format(pm_comm)}
    </div>

    <div class="grid-data-cell net-profit-row-item detail dnone-mobile">
      Netto proprietario
    </div>
    <div class="grid-data-cell net-profit-row-item dnone-mobile">
      € {formatter.format(db_netto)}
    </div>
    <div class="grid-data-cell net-profit-row-item dnone-mobile">
      € {formatter.format(auto_netto)}
    </div>
    <div class="grid-data-cell net-profit-row-item dnone-mobile">
      € {formatter.format(pm_netto)}
    </div>
  </div>
</div>

<style>
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

  .input-grid {
    max-width: 1230px;
    margin: auto;
  }

  .main-container {
    font-family: "Raleway", sans-serif;

    overflow-x: auto;
    margin-top: 2rem;
    background: white;
    border-radius: 10px 10px 0 0;
    border: 1px solid #e5e5e5;

    max-width: 1230px;
    margin: auto;
  }

  .grid-container {
    font-family: "Raleway", sans-serif;
    min-width: 768px;
    display: grid;
    grid-template-columns: 1.5fr 1fr 1fr 1fr;
    border-collapse: collapse;
    align-items: center;
  }

  .grid-header-cell,
  .grid-data-cell {
    font-family: "Raleway", sans-serif;
    padding: 13px;
    text-align: center;
    background-color: white;
    color: #162047;
    align-content: center;
  }

  .grid-data-cell {
    border: 1px solid #e5e5e5;
  }

  .other-header {
    font-family: "Raleway", sans-serif;
    font-weight: bold;
    font-size: 16px;
  }

  .grid-data-cell:nth-child(4n + 1) {
    text-align: left;
    font-weight: bold;
  }

  .net-profit-row-item {
    font-family: "Raleway", sans-serif;
    background-color: #0057ff !important;
    color: white;
    font-weight: 600;
    border: 0;
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
    font-family: "Raleway", sans-serif;
    font-size: 0.85rem;
    margin-bottom: 0.25rem;
    color: #333;
  }

  .input-group input,
  .input-group select {
    font-family: "Raleway", sans-serif;
    width: 100%;
    padding: 0.6rem 0.75rem;
    font-size: 20px;
    border: 1px solid #ccc;
    border-radius: 10px;
  }

  .b0 {
    border: 0 !important;
  }

  .bt-0 {
    border-top: 0 !important;
  }

  .tred {
    color: #ff0037;
  }

  .collapse-cta {
    position: relative;
  }

  .collapse-cta i {
    font-size: 16px;
    color: #0057ff;
    background: none;
    transition: transform 0.5s ease;
    position: absolute;
    right: 28px;
    /* width: 0; */
    pointer-events: none;
  }

  :global(.toggled i) {
    transform: rotate(180deg);
  }

  @media (min-width: 769px) {
    .dnone-desktop {
      display: none;
    }
  }

  @media (max-width: 768px) {
    .grid-container {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      min-width: auto;
    }

    .grid-header-cell,
    .grid-data-cell {
      padding: 13px 6px;
    }

    .grid-header-cell.bt-0:first-of-type {
      grid-column: span 3;
      text-align: center;
    }

    .grid-header-cell.other-header {
      grid-column: span 1;
      font-size: 0.8rem;
      font-weight: bold;
      text-align: center;
    }

    .grid-header-cell.bt-0:not(:first-of-type) {
      grid-column: span 1;
    }

    .grid-data-cell.detail {
      grid-column: span 3;
      font-weight: bold;
      text-align: center;
      background-color: #f5f5f5;
    }

    .grid-data-cell.tred {
      text-align: center;
      color: #ff0037;
      transition: height 0.3s ease;
      height: 52px;
    }

    .net-profit-row-item.detail {
      grid-column: span 3;
      background-color: #0057ff;
      color: white;
      font-weight: bold;
      text-align: center;
    }

    .net-profit-row-item:not(.detail) {
      background-color: #0057ff;
      color: white;
      font-weight: bold;
      text-align: center;
    }

    .main-container {
      overflow-x: hidden;
      padding: 2px;
    }

    .dnone-mobile {
      display: none;
    }

    .dolcebot-logo-img {
      margin: auto;
    }

    :global(.collapsed) {
      visibility: hidden;
      height: 0 !important;
      overflow: hidden;
      padding: 0 !important;
    }
  }
</style>
