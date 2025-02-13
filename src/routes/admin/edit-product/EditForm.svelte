<script>
    let {name, price, buy_price, description, file_id, bucket_id} = $state('')
    import axios from "axios";
    import { API_URL } from "$lib";
    import { Upload } from "phosphor-svelte";
  
    import Loader from "$lib/components/Loader.svelte";
    import Modal from "$lib/components/Modal.svelte";

    let product = $props()
    let loader_toggle = $state(false);
    let file_uploading = $state(false);
    let img_url = $state(product.icon_url);
    let modal_data = $state();
    function showModal(message, cancel, redirect){
        modal_data = {
            "toggle": true,
            "message": message,
            "cancel": cancel,
            "redirect": redirect,
        }
    }

    let files = $state();

    function handleImage(){
        if (files) {
			console.log(files);

			for (const file of files) {
				uploadImage(file)
			}
		}
    }

    async function uploadImage(file)
    {
      file_uploading = true
      const form = new FormData();
      form.append('file', file);

      let options = {
          method: "POST",
          headers: {
              "Content-Type": "application/form-data",
          },
          withCredentials: true,
          data: form
      };

      axios.postForm(API_URL + '/api/files', form, options)
      .then((response) => {
        if(response.status === 201)
        {
          file_id = response.data.data['$id']
          bucket_id = response.data.data['bucketId']
          img_url = response.data.data['file_url']
        }
      })
      .catch(function (error) {
        loader_toggle = false
      })
      .finally( function() {
        file_uploading = false
      });
    }

    async function updateProduct() {
      loader_toggle = true
      let data = {
        'name': name,
        'price': price,
        'buy_price': buy_price,
        'description': description,
        'file_id': file_id,
        'bucket_id': bucket_id
      };
      
      let options = {
          method: "PATCH",
          headers: {
              "Content-Type": "application/json",
          },
          withCredentials: true,
      };

      axios.patch(API_URL + '/api/products/' + product["$id"], data, options)
      .then((response) => {
        if(response.status === 200)
        {
          loader_toggle = false
          showModal("Product detail updated successfully...!", "Continue", '/admin/dashboard')
        }
      })
      .catch(function (error) {
        loader_toggle = false
        if(error.response.status === 422)
        {
          showModal("Failed Adding product.\n\nPlease enter corect details", "Try Again")
        }
        else if (error.response.status < 500)
        {
          showModal(error.response.data.detail, "Try Again")
        }
      })
      .finally(function (){
        loader_toggle = false
      })
    }


</script>

{#if loader_toggle}
  <Loader/>
{:else}
  {#if file_uploading}
    <div class="product-icon py-5">
      <Loader/>
    </div>
  {:else if img_url}
    <img class="product-icon py-5" src={img_url} alt="product icon">
  {/if}
 <div>
  <div class="col-span-full">
    <label for="file-upload" class="text-gray-900 font-semibold mb-2 block">Upload file</label>
    <input id="file-upload"type="file" bind:files={files} onchange={handleImage}
      class="w-full text-gray-400 font-semibold text-sm bg-white border file:cursor-pointer cursor-pointer file:border-0 file:py-2 file:px-4 file:mr-4 file:bg-gray-100 file:hover:bg-gray-200 file:text-gray-500 rounded" />
    <p class="text-xs text-gray-400 mt-2">PNG, JPG SVG, WEBP, and GIF are Allowed.</p>
    </div>

    <div class="col-span-full my-5">
      <label for="street-address" class="block text-sm/6 font-medium text-gray-900">Name *</label>
      <div class="mt-2">
        <input bind:value={name} defaultValue={product.name} type="text" name="street-address" id="street-address" autocomplete="street-address" class="block w-full rounded-md bg-white px-3 py-1.5 text-base text-gray-900 outline-1 -outline-offset-1 outline-gray-300 placeholder:text-gray-400 focus:outline-2 focus:-outline-offset-2 focus:outline-indigo-600 sm:text-sm/6">
      </div>
    </div>

    <div class="mt-10 grid grid-cols-2 gap-x-6 gap-y-8 sm:grid-cols-2">
      <div class="sm:col-span-1">
        <label for="first-name" class="block text-sm/6 font-medium text-gray-900">Sell Price *</label>
        <div class="mt-2">
          <input bind:value={price} defaultValue={product.price} type="text" name="first-name" id="first-name" autocomplete="given-name" class="block w-full rounded-md bg-white px-3 py-1.5 text-base text-gray-900 outline-1 -outline-offset-1 outline-gray-300 placeholder:text-gray-400 focus:outline-2 focus:-outline-offset-2 focus:outline-indigo-600 sm:text-sm/6">
        </div>
      </div>

      <div class="sm:col-span-1">
        <label for="last-name" class="block text-sm/6 font-medium text-gray-900">Buy Price *</label>
        <div class="mt-2">
          <input bind:value={buy_price} defaultValue={product.buy_price} type="text" name="last-name" id="last-name" autocomplete="family-name" class="block w-full rounded-md bg-white px-3 py-1.5 text-base text-gray-900 outline-1 -outline-offset-1 outline-gray-300 placeholder:text-gray-400 focus:outline-2 focus:-outline-offset-2 focus:outline-indigo-600 sm:text-sm/6">
        </div>
      </div>
    </div>

    <div class="col-span-full my-4">
      <label for="about" class="block text-sm/6 font-medium text-gray-900">Description</label>
      <div class="mt-2">
        <textarea bind:value={description} defaultValue={product.description} name="about" id="about" rows="3" class="block w-full rounded-md bg-white px-3 py-1.5 text-base text-gray-900 outline-1 -outline-offset-1 outline-gray-300 placeholder:text-gray-400 focus:outline-2 focus:-outline-offset-2 focus:outline-indigo-600 sm:text-sm/6"></textarea>
      </div>
      <p class="mt-3 text-sm/6 text-gray-300">* Fields marked with are compulsory.</p>
    </div>

    <button class="w-full bg-blue-500 rounded-xs text-slate-100 py-3" onclick={updateProduct}>
      Add
    </button>
</div>
{/if}

<Modal {...modal_data}/>

<style>
  .product-icon{
    max-height: 200px;
    object-fit: contain;
    margin-inline: auto;
  }
</style>