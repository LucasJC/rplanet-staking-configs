<script lang="ts">
  import FormattedPrice from "./FormattedPrice.svelte";
  import * as Staking from "../domain/account-staking";
  import * as Balance from "../domain/balance";
  import { rplanetPrices, waxPrice } from "../domain/currencies";
  import { userStore } from "../domain/user";
  import { currencyBalance } from "../domain/balance";
  import { miningPowerStore } from "../domain/account-staking";
  import { landsIncomeStore } from "../domain/land";

  $: rank = Staking.getRank($miningPowerStore);

  $: monthlyIncome =
    24 *
    30 *
    ($landsIncomeStore + $miningPowerStore * $rplanetPrices.AETHER * $waxPrice);

  $: currentHoldings = Balance.getAccountBalances(
    $currencyBalance,
    $rplanetPrices,
    $waxPrice
  ).usd;
</script>

<main>
  <nav class="section level has-text-center">
    <div class="level-item has-text-centered">
      <div>
        <p class="heading">Account</p>
        <p class="title is-2">{$userStore.account}</p>
      </div>
    </div>
    <div class="level-item has-text-centered">
      <div
        class="pb-2 has-tooltip-bottom has-tooltip-info"
        data-tooltip="You are a {rank.name}"
      >
        <p class="heading">Rank</p>
        <p class="title is-1">{rank.emoji}</p>
      </div>
    </div>
    <div class="level-item has-text-centered">
      <div>
        <p class="heading">Staking Power</p>
        <p class="title is-2">
          <FormattedPrice value={$miningPowerStore} />
          A/h
        </p>
      </div>
    </div>
    <div class="level-item has-text-centered">
      <div
        class="pb-2 has-tooltip-bottom has-tooltip-info"
        data-tooltip="Staking and land passive income in USD"
      >
        <p class="heading">Monthly Income</p>
        <p class="title is-3">
          <FormattedPrice value={monthlyIncome} />
          USD
        </p>
      </div>
    </div>
    <div class="level-item has-text-centered">
      <div
        class="pb-2 has-tooltip-bottom has-tooltip-info"
        data-tooltip="We only consider Aether + Minerls in USD"
      >
        <p class="heading">Current RPlanet Holdings</p>
        <p class="title is-3">
          <FormattedPrice value={currentHoldings} />
          USD
        </p>
      </div>
    </div>
  </nav>
</main>
