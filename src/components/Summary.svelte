<script>
  import data from "../data/data.json";

  export let activeLine = undefined;

  $: activeLineData = data.find((d) => d["Name"] == activeLine);

  let summaryMetrics = [
    { value: "Line Length (mi)", label: "Line Length", suffix: " miles" },
    {
      value: "Dominant Line Voltage (kV)",
      label: "Primary voltage",
      suffix: "kV",
    },
    { value: "States", suffix: "" },
    { value: "Region", suffix: "" },
    {
      value: "Time in Years (NOI to last ROD)",
      label: "Length of EIS review (NOI to last ROD)",
      suffix: " years",
    },
  ];

  function formatValue(value) {
    if (typeof value === "number") {
      return value % 1 === 0 ? value : value.toFixed(1);
    }
    return value;
  }


</script>

{#if activeLineData}
  <div class="g-summary">
    <h2>{activeLine}</h2>

    <div class="g-summary__blocks">
      {#each summaryMetrics as metric, i}
        <div
          class="g-summary__block"
          class:last={summaryMetrics.length - 1 == i}
        >
          <span>{metric.label || metric.value}</span>

          <div>
            {formatValue(activeLineData[metric.value])}
            {#if activeLineData[metric.value] != "-"}
              {metric.suffix}
            {/if}
          </div>
        </div>
      {/each}
    </div>
  </div>
{/if}

<style lang="scss">
  .g-summary {
    margin: 1rem 0px;
    h2 {
      font-size: 2rem;
      font-weight: bold;
      margin-bottom: 0.5rem;
    }

    &__blocks {
      display: grid;
      gap: 0.5rem;
      grid-template-columns: repeat(3, 1fr);

      @include mq("600px", "max-width") {
        grid-template-columns: repeat(2, 1fr);
      }
    }

    &__block {
      display: flex;
      flex-direction: column;
      gap: 0.25rem;

      &.last {
        grid-column: 2 span;
      }

      span {
        font-size: 1rem;
      }
      div {
        font-weight: bold;
        background: #dddddd;
        padding: 1rem;
        display: inline-block;
        border-radius: 5px;
      }
    }
  }
</style>
