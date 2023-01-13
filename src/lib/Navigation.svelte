<script lang="ts">
    import { currentUser, pb } from './pocketbase';

    import Fa from 'svelte-fa';

    import { faEnvelope, faLock } from '@fortawesome/free-solid-svg-icons'

    const logo = { label: "logo", href: ""};

    const navItems = [
        { label: "Sign In", href: "#"},
        { label: "Sign Up", href: "#"},
        { label: "About", href: "#about-section"},
        { label: "Pricing", href: "#pricing-section"},
        { label: "Manage Profile", href: "#"},
    ];

    let email: string;
    let password: string;

    async function signIn() {
        await pb.collection('users').authWithPassword(email, password);
    }

    async function signUp() {
        try {
            const data = {
                email,
                password,
                passwordConfirm: password
            }

            const createdUser = await pb.collection('users').create(data);
            await signIn();
        } catch (err) {
            console.log(err);
        }
    }


    let selectedMenuItem;

    function handleSelectMenuNavigation() {
        console.log(selectedMenuItem);
    }

    function signOut() {
        pb.authStore.clear();
    }

</script>

<nav>
    <div class="navbar bg-base-200">
        <div class="flex-1">
            <a href={logo.href}>{logo.label}</a>
        </div>
        {#if $currentUser}
            <div class="flex-none">
                <ul class="menu menu-horizontal px-1">
                    <li>
                        <button class= "btn btn-primary">Manage Profile</button>
                    </li>
                </ul>
            </div>

            <button class= "btn rounded-md btn-primary" on:click={signOut}>Sign Out</button>
        {:else}
            <div class="flex-none">
                <ul class="menu menu-horizontal px-1">
                    <li>
                        <a href={navItems[2].href}>{navItems[2].label}</a>
                    </li>
                    <div class="divider lg:divider-horizontal"></div> 
                    <li>
                        <a href={navItems[3].href}>{navItems[3].label}</a>
                    </li>
                </ul>
            </div>
            <div class="navbar-end">
                <label for="my-modal-sign-in" class="btn rounded-md">{navItems[0].label}</label>
    
                <!-- Put this part before </body> tag-->
                <form on:submit|preventDefault>
                    <input type="checkbox" id="my-modal-sign-in" class="modal-toggle" />
                    <div class="modal">
                        <div class="modal-box login-form">
                            <h3 class="font-bold text-5xl text-center">Sign In</h3>
                            <div class="divider"></div> 
                            <di class="form-control w-full max-w-xs">
                                <label class="input-group form-field">
                                    <span><Fa icon={faEnvelope} /></span>
                                    <input type="text" placeholder="email" class="input input-bordered input-rounded-none w-full max-w-xs" bind:value={email}/>
                                </label>
                                <label class="input-group form-field">
                                    <span><Fa icon={faLock} /></span>
                                    <input type="password" placeholder="Password" class="input input-bordered input-rounded-none w-full max-w-xs"  bind:value={password}/>
                                </label>
                            </di>
                            <div class="modal-action">
                                <button class= "btn rounded-md" on:click={signIn}>Sign In</button>
                            </div>
                        </div>
                    </div>
                </form>

                <svg xmlns="http://www.w3.org/2000/svg" width="1em" height="3em" preserveAspectRatio="xMidYMid meet" viewBox="0 0 24 24"><path fill="currentColor" d="m7 21l7.9-18H17L9.1 21H7Z"/></svg>
                <label for="my-modal-sign-up" class="btn rounded-md btn-primary">{navItems[1].label}</label>
                <!-- Put this part before </body> tag-->
                <form on:submit>
                <input type="checkbox" id="my-modal-sign-up" class="modal-toggle" />
                <div class="modal">
                    <div class="modal-box login-form">
                        <h3 class="font-bold text-5xl text-center">Sign Up</h3>
                        <div class="divider"></div> 
                        <di class="form-control w-full max-w-xs">
                            <label class="input-group form-field">
                                <span><Fa icon={faEnvelope} /></span>
                                <input type="text" placeholder="email" class="input input-bordered input-rounded-none w-full max-w-xs" bind:value={email}/>
                            </label>
                            <label class="input-group form-field">
                                <span><Fa icon={faLock} /></span>
                                <input type="password" placeholder="Password" class="input input-bordered input-rounded-none w-full max-w-xs" bind:value={password}/>
                            </label>
                        </di>
                        <div class="my-modal-sign-up">
                            <button class= "btn rounded-md btn-primary" on:click={signUp}>Sign Up</button>
                        </div>
                    </div>
                </div>
                </form>
            </div>
        {/if}
    </div>
</nav>
