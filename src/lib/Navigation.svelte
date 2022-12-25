<script lang="ts">
    let showMobileMenu = false;

    const logo = { label: "logo", href: "#"};

    const navItems = [
        { label: "Sign In", href: "#"},
        { label: "Sign Up", href: "#"},
    ];

    const handleMobileIconClick = () => (showMobileMenu = !showMobileMenu)
</script>

<nav>
    <div class="navbar bg-base-100">
        <div class="flex-1">
            <a href={logo.href}>{logo.label}</a>
        </div>
        <div class="flex-none">
            <ul class="menu menu-horizontal px-1">
                <li>
                    <a href={navItems[0].href}>{navItems[0].label}</a>
                </li>
                <div class="divider lg:divider-horizontal"></div> 
                <li>
                    <a href={navItems[1].href}>{navItems[1].label}</a>
                </li>
            </ul>
        </div>
        <div class="navbar-end">
            <label for="my-modal" class="btn rounded-md">{navItems[0].label}</label>

            <!-- Put this part before </body> tag-->
            <input type="checkbox" id="my-modal" class="modal-toggle" />
            <div class="modal">
                <div class="modal-box">
                    <h3 class="font-bold text-lg">Sign In</h3>
                    <div class="divider"></div> 
                    <di class="form-control">
                        <label class="input-group">
                            <span class="fa-regular fa-envelope" />
                            <input type="text" placeholder="Email" class="input input-bordered" />
                        </label>
                        <label class="input-group">
                            <span class="fa-regular fa-envelope" />
                            <input type="password" placeholder="Password" class="input input-bordered" />
                        </label>
                    </di>
                    <div class="modal-action">
                        <label for="my-modal" class="btn rounded-none">Yay!</label>
                    </div>
                </div>
            </div>

            <svg xmlns="http://www.w3.org/2000/svg" width="1em" height="3em" preserveAspectRatio="xMidYMid meet" viewBox="0 0 24 24"><path fill="currentColor" d="m7 21l7.9-18H17L9.1 21H7Z"/></svg>
            <a href={navItems[1].href}><button class="btn-block btn-primary">{navItems[1].label}</button></a>
        </div>
    </div>
</nav>
