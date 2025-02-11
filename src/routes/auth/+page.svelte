<script>
    import { Tabs } from "bits-ui";
    import SigninForm from "./SigninForm.svelte";
    import SigunupForm from "./SigunupForm.svelte";
</script>

<div class="auth-tab">
    <Tabs.Root
      value="signin"
      class="w-[390px] rounded-card border border-slate-300 shadow-[rgba(149, 157, 165, 0.2) 0px 8px 24px] bg-background-alt p-3 shadow-card"
    >
      <Tabs.List
        class="grid w-full grid-cols-2 gap-1 rounded-9px bg-dark-10 p-1 text-sm font-semibold leading-[0.01em] shadow-mini-inset border-neutral-600/30 bg-slate-100"
      >
        <Tabs.Trigger
          value="signin"
          class="h-8 rounded-[7px] bg-transparent py-2 data-[state=active]:bg-white data-[state=active]:shadow-mini dark:data-[state=active]:bg-muted"
          >Sign In</Tabs.Trigger>
        <Tabs.Trigger
          value="signup"
          class="h-8 rounded-[7px] bg-transparent py-2 data-[state=active]:bg-white data-[state=active]:shadow-mini dark:data-[state=active]:bg-muted"
          >Sign Up</Tabs.Trigger
        >
      </Tabs.List>
      <Tabs.Content value="signin" class="pt-3">
        <SigninForm/>
      </Tabs.Content>
      <Tabs.Content value="signup" class="pt-3">
        <SigunupForm/>
      </Tabs.Content>
    </Tabs.Root>
  </div>

  <style>
    .auth-tab{
        padding-block: 4rem;
        display: flex;
        justify-content: center;
    }
  </style>