<script lang="ts">
import { t } from "@transifex/native";

import { getContext } from "svelte";

import { CddaData, singular, singularName } from "../data";
import type { Bionic } from "../types";
import ThingLink from "./ThingLink.svelte";

export let item: Bionic;
const data = getContext<CddaData>("data");
const _context = "Bionic";
</script>

<h1>{t("Bionic")}: {singularName(item)}</h1>
<section>
  <h1>{t("General", { _context, _comment: "Section heading" })}</h1>
  <dl>
    <dt>{t("Occupied Body Parts", { _context })}</dt>
    <dd>
      {#if item.occupied_bodyparts}
        <ul class="comma-separated">
          {#each item.occupied_bodyparts as [bp_id, slots]}
            <li>
              {singularName(data.byId("body_part", bp_id))} ({slots}
              {slots === 1 ? "slot" : "slots"})
            </li>
          {/each}
        </ul>
      {:else}
        <em>{t("none")}</em>
      {/if}
    </dd>
    {#if item.encumbrance}
      <dt>{t("Encumbrance", { _context })}</dt>
      <dd>
        <ul class="comma-separated">
          {#each item.encumbrance as [bp_id, encum]}
            <li>{singularName(data.byId("body_part", bp_id))} ({encum})</li>
          {/each}
        </ul>
      </dd>
    {/if}
    {#if item.stat_bonus}
      <dt>{t("Stat Bonuses", { _context })}</dt>
      <dd>
        <ul class="comma-separated">
          {#each item.stat_bonus as [stat_id, bonus]}
            <li>{bonus >= 0 ? `+${bonus}` : `–${-bonus}`} {stat_id}</li>
          {/each}
        </ul>
      </dd>
    {/if}
    {#if item.cant_remove_reason}
      <dt>{t("Removable", { _context })}</dt>
      <dd>{t("no")}</dd>
    {/if}
    {#if item.act_cost}
      <dt>{t("Activation Cost", { _context })}</dt>
      <dd>{item.act_cost}</dd>
    {/if}
    {#if item.trigger_cost}
      <dt>{t("Trigger Cost", { _context })}</dt>
      <dd>{item.trigger_cost}</dd>
    {/if}
    {#if item.react_cost}
      <dt>{t("Power Over Time", { _context })}</dt>
      <dd>{item.react_cost}</dd>
    {/if}
    {#if (item.vitamin_absorb_mod ?? 1) !== 1}
      <dt>{t("Vitamin Absorption Modifier", { _context })}</dt>
      <dd>{(item.vitamin_absorb_mod * 100).toFixed(0)}%</dd>
    {/if}
    {#if item.fuel_options?.length}
      <dt>{t("Fuel", { _context })}</dt>
      <dd>
        <ul class="comma-separated">
          {#each item.fuel_options as material_id}
            <li><ThingLink type="material" id={material_id} /></li>
          {/each}
        </ul>
      </dd>
    {/if}
    {#if item.fuel_capacity}
      <dt>{t("Fuel Capacity", { _context })}</dt>
      <dd>{item.fuel_capacity}</dd>
    {/if}
    {#if item.fuel_efficiency}
      <dt>{t("Fuel Efficiency", { _context })}</dt>
      <dd>{item.fuel_efficiency * 100}%</dd>
    {/if}
    {#if item.passive_fuel_efficiency}
      <dt>{t("Passive Fuel Efficiency", { _context })}</dt>
      <dd>{item.passive_fuel_efficiency * 100}%</dd>
    {/if}
    {#if item.exothermic_power_gen}
      <dt>{t("Emits Heat", { _context })}</dt>
      <dd>{t("yes")}</dd>
    {/if}
    {#if item.power_trickle}
      <dt>{t("Power Trickle", { _context })}</dt>
      <dd>{item.power_trickle}</dd>
    {/if}
    {#if item.time}
      <dt>{t("Charge Time", { _context })}</dt>
      <dd>{item.time} turn{item.time !== 1 ? "s" : ""}</dd>
    {/if}
    {#if item.capacity}
      <dt>{t("Capacity", { _context })}</dt>
      <dd>{item.capacity}</dd>
    {/if}
    {#if item.fake_weapon}
      <dt>{t("Acts As", { _context })}</dt>
      <dd><ThingLink type="item" id={item.fake_weapon} /></dd>
    {/if}
    {#if item.toggled_pseudo_items?.length}
      <dt>{t("Acts As", { _context })}</dt>
      <dd>
        <ul class="comma-separated">
          {#each item.toggled_pseudo_items as item_id}
            <li><ThingLink type="item" id={item_id} /></li>
          {/each}
        </ul>
      </dd>
    {/if}
    {#if item.passive_pseudo_items?.length}
      <dt>{t("Acts As", { _context })}</dt>
      <dd>
        <ul class="comma-separated">
          {#each item.passive_pseudo_items as item_id}
            <li><ThingLink type="item" id={item_id} /></li>
          {/each}
        </ul>
      </dd>
    {/if}
    {#if item.included_bionics?.length}
      <dt>{t("Includes", { _context })}</dt>
      <dd>
        <ul class="comma-separated">
          {#each item.included_bionics as bionic_id}
            <li><ThingLink type="bionic" id={bionic_id} /></li>
          {/each}
        </ul>
      </dd>
    {/if}
    <dt>{t("Duplicates Allowed", { _context })}</dt>
    <dd>{item.dupes_allowed ? t("yes") : t("no")}</dd>
    <dt>{t("Flags")}</dt>
    <dd>
      {#if item.flags?.length}
        <ul class="comma-separated">
          {#each item.flags as flag}
            <li><ThingLink type="json_flag" id={flag} /></li>
          {/each}
        </ul>
      {:else}
        <em>{t("none")}</em>
      {/if}
    </dd>
    {#if item.active_flags}
      <dt>{t("Flags When Active", { _context })}</dt>
      <dd>
        <ul class="comma-separated">
          {#each item.active_flags as flag}
            <li><ThingLink type="json_flag" id={flag} /></li>
          {/each}
        </ul>
      </dd>
    {/if}
    {#if item.mutation_conflicts}
      <dt>{t("Conflicts With Mutations", { _context })}</dt>
      <dd>
        <ul class="comma-separated">
          {#each item.mutation_conflicts as mutation_id}
            {#if data.byId("mutation", mutation_id)}
              <li><ThingLink type="mutation" id={mutation_id} /></li>
            {/if}
          {/each}
        </ul>
      </dd>
    {/if}
    {#if item.canceled_mutations}
      <dt>{t("Removes Mutations", { _context })}</dt>
      <dd>
        <ul class="comma-separated">
          {#each item.canceled_mutations as mutation_id}
            {#if data.byId("mutation", mutation_id)}
              <li><ThingLink type="mutation" id={mutation_id} /></li>
            {/if}
          {/each}
        </ul>
      </dd>
    {/if}
  </dl>
  <p style="color: var(--cata-color-gray)">{singular(item.description)}</p>
</section>
