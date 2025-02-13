<script>
    import { Menubar } from 'bits-ui';
    import { House } from "phosphor-svelte";
    import { flyAndScale } from "$lib/bits-ui/utils/transitions";
    import { goto } from '$app/navigation';
    import { page } from '$app/state';
    import axios from 'axios';
    import { API_URL } from '$lib';

    import Modal from '$lib/components/Modal.svelte';

    let modal_data = $state();
    function showModal(message, cancel, redirect){
        modal_data = {
            "toggle": true,
            "message": message,
            "cancel": cancel,
            "redirect": redirect,
        }
    }
    function logout()
    {
      axios.delete(API_URL + '/auth/token/', {withCredentials: true})
      .then((response) => {
        if(response.status === 200)
        {
          showModal('Logout successful!', 'Continue', '/')
        }
      })
      .catch(function (error) {
        showModal("Error", 'Continue')
      })
    }

</script>

<Modal {...modal_data}/>

<Menubar.Root
  class="flex flex-colum items-center rounded-10px border border-slate-300 py-[.8rem] bg-background-alt shadow-mini"
>
  <a href="/" class="px-2.5 ms-3 items-center me-[auto] text-xs font-bold text-white bg-blue-600 flex gap-2 border px-5 py-2 rounded" >
    <!-- <House color="white" class="size-5" weight="duotone"/> -->
    TechinalSolution
  </a>
  <Menubar.Menu>
    <Menubar.Trigger
      class="mr-[20px] inline-flex h-10 cursor-default items-center justify-center rounded-[9px] px-3 text-sm font-medium !ring-0 !ring-transparent data-[highlighted]:bg-muted data-[state=open]:bg-muted"
      >Menu</Menubar.Trigger
    >
    <Menubar.Content
      class="z-50 w-full max-w-[220px] rounded-md border border-slate-300 bg-white px-1 py-1.5 shadow-popover"
      transition={flyAndScale}
      align="start"
      sideOffset={3}
    >
      <Menubar.RadioGroup >

        <Menubar.RadioItem
          class="flex h-10 select-none items-center rounded-button py-3 pl-3 pr-1.5 text-sm font-medium !ring-0 !ring-transparent data-[highlighted]:bg-muted"
          value="Admin Dashboard"
          onclick={() => {goto('/admin/dashboard')}}
        >
          Admin Dashboard
          <Menubar.RadioIndicator class="ml-auto">
          </Menubar.RadioIndicator>
        </Menubar.RadioItem>

        <Menubar.RadioItem
          class="flex h-10 select-none items-center rounded-button py-3 pl-3 pr-1.5 text-sm font-medium !ring-0 !ring-transparent data-[highlighted]:bg-muted"
          value="Profile"
          onclick={() => {goto('/profile')}}
        >
          Profile
          <Menubar.RadioIndicator class="ml-auto">
          </Menubar.RadioIndicator>
        </Menubar.RadioItem>

        <Menubar.RadioItem
          class="flex h-10 select-none items-center rounded-button py-3 pl-3 pr-1.5 text-sm font-medium !ring-0 !ring-transparent data-[highlighted]:bg-muted"
          value="Login / Signup"
          onclick={() => {goto('/auth')}}
        >
          Login / Signup
          <Menubar.RadioIndicator class="ml-auto">
          </Menubar.RadioIndicator>
        </Menubar.RadioItem>


      </Menubar.RadioGroup>
      <Menubar.Separator />
        <Menubar.Item class="flex h-10 select-none items-center rounded-button py-3 pl-3 pr-1.5 text-sm font-medium !ring-0 !ring-transparent data-[highlighted]:bg-muted" onclick={() => {window.location = "/api/docs"}}>
            Api Docs
        </Menubar.Item>
        <Menubar.Item class="flex h-10 select-none items-center rounded-button py-3 pl-3 pr-1.5 text-sm font-medium !ring-0 !ring-transparent data-[highlighted]:bg-muted" onclick={logout} >
            Log Out
        </Menubar.Item>
    </Menubar.Content>
  </Menubar.Menu>
</Menubar.Root>