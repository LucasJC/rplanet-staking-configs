<script lang="ts">
  import { rplanetPrices, waxPrice } from "../domain/currencies";
  import { currencyBalance } from "../domain/balance";
  import * as Balance from "../domain/balance";
  import FormattedPrice from "./FormattedPrice.svelte";

  let balances: Array<Balance.CalculatedBalance> = [];
  let totalWax: number = 0;
  let totalUSD: number = 0;

  $: {
    const accountBalance = Balance.getAccountBalances(
      $currencyBalance,
      $rplanetPrices,
      $waxPrice
    );
    balances = accountBalance.balances;
    totalWax = accountBalance.wax;
    totalUSD = accountBalance.usd;
  }
</script>

{#if balances.length > 0}
  <div class="section">
    <p class="title is-4">Account Holdings</p>
    <table class="table is-bordered is-narrow is-fullwidth has-text-centered">
      <tr>
        <th>Currency</th>
        <th>Amount</th>
        <th>in Wax</th>
        <th>in USD</th>
      </tr>
      {#each balances as b}
        <tr>
          <td>
            <div
              class="image is-24x24 is-hcentered has-tooltip-right"
              data-tooltip={b.currency}
            >
              <img
                src="image/{b.currency.toLowerCase()}.png"
                alt={b.currency}
              />
            </div>
          </td>
          <td class="has-text-right"><FormattedPrice value={b.amount} /></td>
          <td class="has-text-right"><FormattedPrice value={b.waxAmount} /></td>
          <td class="has-text-right"><FormattedPrice value={b.usdAmount} /></td>
        </tr>
      {/each}
      <tr
        class="has-text-weight-bold has-background-info-light has-text-info-dark"
      >
        <td>Total</td>
        <td> - </td>
        <td class="has-text-right"><FormattedPrice value={totalWax} /></td>
        <td class="has-text-right"><FormattedPrice value={totalUSD} /></td>
      </tr>
    </table>
  </div>
{/if}
