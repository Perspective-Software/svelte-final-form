<script>
  import { getContext, onDestroy } from "svelte";
  import { FORM } from "svelte-final-form/src/Form.svelte";
  import { getFormSubscriptionItems } from "./getFormSubscriptions";

  export let subscription = getFormSubscriptionItems();
  export let onChange;

  let formState = {};
  let unsubscribe;

  const form = getContext(FORM);

  $: {
    unsubscribe && unsubscribe();
    unsubscribe = form.subscribe((nextFormState) => {
      onChange && onChange(nextFormState);
      formState = nextFormState;
    }, subscription);
  }

  onDestroy(() => {
    unsubscribe && unsubscribe();
  });
</script>

{#if $$slots.default}
  <slot {formState} />
{/if}
