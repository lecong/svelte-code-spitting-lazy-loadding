<script>
  import Link from "$lib/components/link/Link.svelte";
  import Route from "$lib/components/route/Route.svelte";
  import Router from "$lib/components/route/Router.svelte";
  import Home from "./routes/Home.svelte";

  const delayModuleLoad = (module) =>
    new Promise((res) => setTimeout(() => res(module), Math.random() * 2000));

  const Page1 = () => import("./routes/Page1.svelte").then(delayModuleLoad);
  const Page2 = () => import("./routes/Page2.svelte").then(delayModuleLoad);
  const Page3 = () => import("./routes/Page3.svelte").then(delayModuleLoad);

  export let url = "";
</script>

<Router {url}>
  <div class="flex bg-gray-100 text-gray-900">
    <aside
      class="flex h-screen w-20 flex-col items-center border-r border-gray-200 bg-white"
    >
      <div
        class="flex h-[4.5rem] w-full items-center justify-center border-b border-gray-200 p-2"
      >
        <img
          alt="Svelte logo"
          src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcThsapwuIZ2JPUVRaWSoX_xoEIOHWxneY7EupS8gsFriA&s"
        />
      </div>
      <nav class="flex flex-1 flex-col gap-y-4 pt-10">
        <Link
          to="/"
          class="text-gary-400 group relative rounded-xl p-2 hover:bg-gray-50"
        >
          Home
        </Link>
        <Link
          to="/page-1"
          class="text-gary-400 group relative rounded-xl p-2 hover:bg-gray-50"
        >
          Page 1
        </Link>
        <Link
          to="/page-2"
          class="text-gary-400 group relative rounded-xl p-2 hover:bg-gray-50"
        >
          Page 2
        </Link>
        <Link
          to="/page-3"
          class="text-gary-400 group relative rounded-xl p-2 hover:bg-gray-50"
        >
          Page 4
        </Link>
      </nav>

      <div class="flex flex-col items-center gap-y-4 py-10">
        <button
          class="group relative rounded-xl p-2 text-gray-400 hover:bg-gray-100"
        >
          <svg
            width="24"
            height="24"
            class="h-6 w-6 stroke-current"
            viewBox="0 0 24 24"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M12 21C16.9706 21 21 16.9706 21 12C21 7.02944 16.9706 3 12 3C7.02944 3 3 7.02944 3 12C3 16.9706 7.02944 21 12 21Z"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
            />
            <path
              d="M12 16H12.01M12 8V12V8Z"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
            />
          </svg>
        </button>

        <button class="mt-2 rounded-full bg-gray-100">
          <img
            class="h-10 w-10 rounded-full"
            src="https://avatars.githubusercontent.com/u/35387401?v=4"
            alt=""
          />
        </button>
      </div>
    </aside>
    <main>
      <Route path="/page-1" component={Page1} />
      <Route path="/page-2" component={Page2} />
      <Route path="/page-3" component={Page3} />
      <Route path="/"><Home /></Route>
    </main>
  </div>
</Router>
