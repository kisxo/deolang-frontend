<script>
    let {name, price, buy_price, description, file_id, bucket_id} = $state('')
    import axios from "axios";
    import { API_URL } from "$lib";
    import { Upload } from "phosphor-svelte";
  
    import Loader from "$lib/components/Loader.svelte";
    import Modal from "$lib/components/Modal.svelte";

    let loader_toggle = $state(false)
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
        }
      })
      .catch(function (error) {
        loader_toggle = false
      });
    }

    async function addProduct() {
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
          method: "POST",
          headers: {
              "Content-Type": "application/json",
          },
          withCredentials: true,
      };

      axios.post(API_URL + '/api/products', data, options)
      .then((response) => {
        if(response.status === 201)
        {
          showModal("Added new product successfully...!", "Continue", '/admin/dashboard')
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

      });
    }


</script>

{#if loader_toggle}
  <Loader/>
{:else}
 <div>
  <div class="col-span-full">
      <label for="cover-photo" class="block text-sm font-medium text-gray-900">Product Icon</label>
      <div class="mt-2 flex justify-center rounded-lg border border-dashed border-gray-900/25 px-6 py-10">
        <div class="text-center">
          <svg class="mx-auto size-12 text-gray-300" viewBox="0 0 24 24" fill="currentColor" aria-hidden="true" data-slot="icon">
            <path fill-rule="evenodd" d="M1.5 6a2.25 2.25 0 0 1 2.25-2.25h16.5A2.25 2.25 0 0 1 22.5 6v12a2.25 2.25 0 0 1-2.25 2.25H3.75A2.25 2.25 0 0 1 1.5 18V6ZM3 16.06V18c0 .414.336.75.75.75h16.5A.75.75 0 0 0 21 18v-1.94l-2.69-2.689a1.5 1.5 0 0 0-2.12 0l-.88.879.97.97a.75.75 0 1 1-1.06 1.06l-5.16-5.159a1.5 1.5 0 0 0-2.12 0L3 16.061Zm10.125-7.81a1.125 1.125 0 1 1 2.25 0 1.125 1.125 0 0 1-2.25 0Z" clip-rule="evenodd" />
          </svg>
          <div class="mt-4 flex text-sm/6 text-gray-600">
            <label for="file-upload" class="relative cursor-pointer rounded-md bg-white font-semibold text-indigo-600 focus-within:ring-2 focus-within:ring-indigo-600 focus-within:ring-offset-2 focus-within:outline-hidden hover:text-indigo-500">
              <span>Upload a file</span>
              <input id="file-upload" name="file-upload" type="file" bind:files={files} onchange={handleImage} class="sr-only">
            </label>
            <p class="pl-1">or drag and drop</p>
          </div>
          <p class="text-xs/5 text-gray-600">PNG, JPG, GIF up to 5MB</p>
        </div>
      </div>
    </div>

    <div class="col-span-full my-5">
      <label for="street-address" class="block text-sm/6 font-medium text-gray-900">Name *</label>
      <div class="mt-2">
        <input bind:value={name} type="text" name="street-address" id="street-address" autocomplete="street-address" class="block w-full rounded-md bg-white px-3 py-1.5 text-base text-gray-900 outline-1 -outline-offset-1 outline-gray-300 placeholder:text-gray-400 focus:outline-2 focus:-outline-offset-2 focus:outline-indigo-600 sm:text-sm/6">
      </div>
    </div>

    <div class="mt-10 grid grid-cols-2 gap-x-6 gap-y-8 sm:grid-cols-2">
      <div class="sm:col-span-1">
        <label for="first-name" class="block text-sm/6 font-medium text-gray-900">Sell Price *</label>
        <div class="mt-2">
          <input bind:value={price} type="text" name="first-name" id="first-name" autocomplete="given-name" class="block w-full rounded-md bg-white px-3 py-1.5 text-base text-gray-900 outline-1 -outline-offset-1 outline-gray-300 placeholder:text-gray-400 focus:outline-2 focus:-outline-offset-2 focus:outline-indigo-600 sm:text-sm/6">
        </div>
      </div>

      <div class="sm:col-span-1">
        <label for="last-name" class="block text-sm/6 font-medium text-gray-900">Buy Price *</label>
        <div class="mt-2">
          <input bind:value={buy_price} type="text" name="last-name" id="last-name" autocomplete="family-name" class="block w-full rounded-md bg-white px-3 py-1.5 text-base text-gray-900 outline-1 -outline-offset-1 outline-gray-300 placeholder:text-gray-400 focus:outline-2 focus:-outline-offset-2 focus:outline-indigo-600 sm:text-sm/6">
        </div>
      </div>
    </div>

    <div class="col-span-full my-4">
      <label for="about" class="block text-sm/6 font-medium text-gray-900">Description</label>
      <div class="mt-2">
        <textarea bind:value={description} name="about" id="about" rows="3" class="block w-full rounded-md bg-white px-3 py-1.5 text-base text-gray-900 outline-1 -outline-offset-1 outline-gray-300 placeholder:text-gray-400 focus:outline-2 focus:-outline-offset-2 focus:outline-indigo-600 sm:text-sm/6"></textarea>
      </div>
      <p class="mt-3 text-sm/6 text-gray-300">* Fields marked with are compulsory.</p>
    </div>

    <button class="w-full bg-blue-500 rounded-xs text-slate-100 py-3" onclick={addProduct}>
      Add
    </button>
</div>
{/if}

<Modal {...modal_data}/>