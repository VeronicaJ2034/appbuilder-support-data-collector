<script lang="ts">
  import {
    SUPPORT_DATA_ITEMS,
    toggleSupportDataItem,
    type SupportDataItemId,
  } from "./index";

  interface Props {
    requestedItems?: readonly SupportDataItemId[];
    selectedItems?: SupportDataItemId[];
  }

  let { requestedItems = [], selectedItems = $bindable([...requestedItems]) }: Props = $props();
  const requested = $derived(new Set(requestedItems));

  function toggleItem(id: SupportDataItemId): void {
    selectedItems = toggleSupportDataItem(selectedItems, id);
  }
</script>

<section class="space-y-3" aria-labelledby="support-data-selection-heading">
  <div>
    <h2 id="support-data-selection-heading" class="text-lg font-semibold">Support data to include</h2>
    <p class="mt-1 text-sm text-base-content/70">
      Select only the information you want to share. Requested items are recommendations, not
      permission to collect data.
    </p>
  </div>

  <div class="space-y-2">
    {#each SUPPORT_DATA_ITEMS as item (item.id)}
      {@const isSelected = selectedItems.includes(item.id)}
      <label class="flex cursor-pointer items-start gap-3 rounded-box border border-base-300 bg-base-100 p-3">
        <input
          class="dy-checkbox dy-checkbox-sm mt-0.5"
          type="checkbox"
          checked={isSelected}
          onchange={() => toggleItem(item.id)}
          aria-label={item.displayName}
        />
        <span class="min-w-0 flex-1">
          <span class="flex flex-wrap items-center gap-2 font-medium">
            {item.displayName}
            {#if requested.has(item.id)}
              <span class="dy-badge dy-badge-sm dy-badge-outline">Requested</span>
            {/if}
            {#if item.sensitive}
              <span class="dy-badge dy-badge-sm dy-badge-warning">Sensitive</span>
            {/if}
          </span>
          <span class="mt-1 block text-sm text-base-content/70">{item.description}</span>
        </span>
      </label>
    {/each}
  </div>

  <p class="text-sm text-base-content/70">
    {selectedItems.length} {selectedItems.length === 1 ? "item" : "items"} selected.
  </p>
</section>
