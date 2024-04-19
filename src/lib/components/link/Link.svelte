<script>
  import { createEventDispatcher, getContext } from "svelte";
  import { HISTORY, LOCATION, ROUTER } from "../../contexts";
  import { resolve, shouldNavigate } from "../../utils";

  export let to = "#";
  export let replace = false;
  export let state = {};
  export let getProps = () => ({});
  export let preserveScroll = false;

  const location = getContext(LOCATION);
  const { base } = getContext(ROUTER);
  const { navigate } = getContext(HISTORY);
  const dispatch = createEventDispatcher();

  let href, isPartiallyCurrent, isCurrent, props;
  $: href = resolve(to, $base.uri);
  $: isPartiallyCurrent = $location.pathname.startsWith(href);
  $: isCurrent = href === $location.pathname;
  $: ariaCurrent = isCurrent ? "page" : undefined;
  // @ts-ignore
  $: props = getProps({
    location: $location,
    href,
    isPartiallyCurrent,
    isCurrent,
    existingProps: $$restProps,
  });

  const onClick = (event) => {
    dispatch("click", event);
    if (shouldNavigate(event)) {
      event.preventDefault();
      // Don't push another entry to the history stack when the user
      // clicks on a Link to the page they are currently on.
      const shouldReplace = $location.pathname === href || replace;
      navigate(href, { state, replace: shouldReplace, preserveScroll });
    }
  };
</script>

<a
  {href}
  aria-current={ariaCurrent}
  on:click={onClick}
  {...props}
  {...$$restProps}
  class:bg-gray-100={!!ariaCurrent}
>
  <slot name="icon">
    <svg
      class="h-6 w-6 stroke-current group-hover:text-blue-600"
      class:text-blue-600={!!ariaCurrent}
      width="24"
      height="24"
      viewBox="0 0 24 24"
      fill="none"
      xmlns="http://www.w3.org/2000/svg"
    >
      <path
        d="M18 4H6C4.89543 4 4 4.89543 4 6V18C4 19.1046 4.89543 20 6 20H18C19.1046 20 20 19.1046 20 18V6C20 4.89543 19.1046 4 18 4Z"
        stroke-width="2"
        stroke-linecap="round"
        stroke-linejoin="round"
      />
      <path
        d="M12 9V15M9 12H15H9Z"
        stroke-width="2"
        stroke-linecap="round"
        stroke-linejoin="round"
      />
    </svg>
  </slot>

  <div class="absolute inset-y-0 left-12 hidden items-center group-hover:flex">
    <div
      class="relative whitespace-nowrap rounded-md bg-white px-4 py-2 text-sm font-semibold text-gray-900 drop-shadow-lg"
    >
      <div class="absolute inset-0 -left-1 flex items-center">
        <div class="h-2 w-2 rotate-45 bg-white"></div>
      </div>
      <slot active={!!ariaCurrent} />
    </div>
  </div>
</a>
