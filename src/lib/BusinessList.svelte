<script lang="ts">
    import { onMount, onDestroy } from 'svelte';
    import { currentUser, pb } from './pocketbase';
    import Fa from 'svelte-fa';

    import { faSquarePlus } from '@fortawesome/free-solid-svg-icons'

    let businesses = [];
    let unsubscribe: () => void;

    onMount(async () => {
        const resultList = await pb.collection('business').getList();

        businesses = resultList.items;

        unsubscribe = await pb.collection('businesses').subscribe('*', async ({ action, record }) => {
            if (action === 'create') {
                const owner = await pb.collection('users').getOne(record.owner);
                record.expand = { owner }
                businesses = [...businesses, record];
            }

            if (action === 'delete') {
                businesses = businesses.filter((m) => m.id !== record.id)
            }
        });

        console.log(businesses)
    });

    onDestroy(() => {
        unsubscribe?.();
    });

    let name: string;
    let number: string;
    let description: string;

    async function createBusiness() {
        const data = {
            owner: $currentUser.id,
            name: name,
            phone_number: number,
            description: description
        }
        await pb.collection('business').create(data);
    }
</script>

<div class="business-showcase">
    {#if businesses.length}
        {#each businesses as business}
            <div class="card w-96 bg-base-100 shadow-xl flex flex-col business-item transition duration-300 hover:bg-base-300">
                <figure><img src={pb.getFileUrl(business, business.logo, { 'thumb': '100x100'})} alt="logo" /></figure>
                <div class="card-body items-center text-center">
                    <h2 class="card-title">{business.name}</h2>
                    <p>{business.description}</p>
                </div>
            </div>
        {/each}
        <div class="card w-96 bg-base-100 shadow-xl">
            <div class="card-body">
                <label for="my-modal-business-create" class="btn h-96 inline-flex items-center justify-center"><Fa icon={faSquarePlus} size='7x' /></label>
                <!-- <form on:submit|preventDefault={createBusiness}> -->
                    <input type="checkbox" id="my-modal-business-create" class="modal-toggle" />
                    <div class="modal">
                        <div class="modal-box login-form">
                            <form on:submit={createBusiness}>
                                <label for="my-modal-business-create" class="btn btn-sm btn-circle absolute right-2 top-2">✕</label>
                                <h3 class="font-bold text-5xl text-center">Business Details</h3>
                                <div class="divider"></div> 
                                <div class="form-control w-full max-w-xs">
                                    <label class="input-group form-field">
                                        <input type="text" placeholder="Business Name" class="input input-bordered input-rounded-lg w-full max-w-xs" bind:value={name}/>
                                    </label>
                                    <label class="input-group form-field">
                                        <input type="text" placeholder="Phone Number" class="input input-bordered input-rounded-lg w-full max-w-xs" bind:value={number}/>
                                    </label>
                                    <textarea id="message" rows="4" class="block p-2.5 w-full text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" placeholder="Business Description" bind:value={description}></textarea>
                                </div>
                                
                                <div class="modal-action">
                                    <button type="submit" class="btn rounded-md">Add Business</button>
                                </div>
                                <!-- <button type="submit" class="btn rounded-md">Add Business</button> -->
                            </form>    
                        </div>
                    </div>
                <!-- </form> -->
                <p class="font-bold">Click the '+' to add a business to your profile</p>
            </div>
        </div>
        {:else}
        <div class="card w-96 bg-base-100 shadow-xl">
            <div class="card-body">
                <label for="my-modal-business-create" class="btn h-96 inline-flex items-center justify-center"><Fa icon={faSquarePlus} size='7x' /></label>
                <!-- <form on:submit|preventDefault={createBusiness}> -->
                    <input type="checkbox" id="my-modal-business-create" class="modal-toggle" />
                    <div class="modal">
                        <div class="modal-box login-form">
                            <form on:submit|preventDefault={createBusiness}>
                                <label for="my-modal-business-create" class="btn btn-sm btn-circle absolute right-2 top-2">✕</label>
                                <h3 class="font-bold text-5xl text-center">Business Details</h3>
                                <div class="divider"></div> 
                                <div class="form-control w-full max-w-xs">
                                    <label class="input-group form-field">
                                        <input type="text" placeholder="Business Name" class="input input-bordered input-rounded-lg w-full max-w-xs" bind:value={name}/>
                                    </label>
                                    <label class="input-group form-field">
                                        <input type="text" placeholder="Phone Number" class="input input-bordered input-rounded-lg w-full max-w-xs" bind:value={number}/>
                                    </label>
                                    <textarea id="message" rows="4" class="block p-2.5 w-full text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" placeholder="Business Description" bind:value={description}></textarea>
                                </div>
                                
                                <div class="modal-action">
                                    <button type="submit" class="btn rounded-md">Add Business</button>
                                </div>
                                <!-- <button type="submit" class="btn rounded-md">Add Business</button> -->
                            </form>    
                        </div>
                    </div>
                <!-- </form> -->
                <p class="font-bold">Click the '+' to add a business to your profile</p>
            </div>
        </div>
    {/if}
</div>