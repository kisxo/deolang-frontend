<script lang="ts">
    import { Dialog, Label, Separator } from "bits-ui";
    import { fade } from "svelte/transition";
    import { flyAndScale } from "$lib/bits-ui/utils/transitions";
    import { goto } from "$app/navigation";
    let { toggle, message, cancel="Close", redirect='', callback} = $props();

    function check_redirect(){
      if(redirect != '')
      {
        goto(redirect);
      }
    }
</script>
   
  <Dialog.Root bind:open={toggle} closeOnOutsideClick={false}>
    <Dialog.Portal>
      <Dialog.Overlay transition={fade} transitionConfig={{ duration: 150 }} class="fixed inset-0 z-50 bg-black/80"/>
      <Dialog.Content transition={flyAndScale} class="fixed text-center flex flex-col justify-center gap-[3rem] bg-white border-none rounded-md py-[3rem] left-[50%] top-[50%] z-50 w-full max-w-[80%] translate-x-[-50%] translate-y-[-50%] rounded-card-lg border bg-background p-5 shadow-popover outline-none sm:max-w-[490px] md:w-full">
        <Dialog.Description class="text-sm w-[75%] mx-auto text-foreground-alt">
            {message}
        </Dialog.Description>
        
        <Dialog.Close onclick={() => {check_redirect; callback()}} class="bg-black text-white rounded-xm mx-auto w-[60%] py-[.5rem] focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-foreground focus-visible:ring-offset-2 focus-visible:ring-offset-background active:scale-98">
            <span>{cancel}</span>
        </Dialog.Close>
      </Dialog.Content>
    </Dialog.Portal>
  </Dialog.Root>