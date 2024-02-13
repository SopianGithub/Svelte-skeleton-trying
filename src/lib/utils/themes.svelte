<script lang="ts">

    let classMode : string = 'dark';
    let activeTheme: string = 'skeleton';
    let isDropdownOpen: boolean = false;

    const themes: any = [
        {
            name: 'skeleton',
            icon: '💀',
            label: 'Skeleton'
        },
        {
            name: 'wintry',
            icon: '🌨️',
            label: 'Wintry'
        },
        {
            name: 'modern',
            icon: '🤖',
            label: 'Modern'
        },
        {
            name: 'rocket',
            icon: '🚀',
            label: 'Rocket'
        },
        {
            name: 'seafoam',
            icon: '🧜‍♀️',
            label: 'Seafoam'
        },
        {
            name: 'vintage',
            icon: '📺',
            label: 'Vintage'
        },
        {
            name: 'sahara',
            icon: '🏜️',
            label: 'Sahara'
        },
        {
            name: 'hamlindigo',
            icon: '👔',
            label: 'Hamlindigo'
        },
        {
            name: 'gold-nouveau',
            icon: '💫',
            label: 'Gold Nouveau'
        },
        {
            name: 'crimson',
            icon: '⭕',
            label: 'Crimson'
        }
    ]

    function changeThemes(theme: string): void {
        activeTheme = theme;
		document.getElementsByTagName('body')[0].setAttribute('data-theme', theme);
        getActiveTheme(theme);
	}

    $: getActiveTheme = (theme: string): string|null => {
        if(activeTheme == theme) {
            return 'bg-primary-active-token';
        }
        return null;
    }

    const handleDropdownFocusLoss = ({ relatedTarget, currentTarget }) => {
        // use "focusout" event to ensure that we can close the dropdown when clicking outside or when we leave the dropdown with the "Tab" button
        if (relatedTarget instanceof HTMLElement && currentTarget.contains(relatedTarget)) return // check if the new focus target doesn't present in the dropdown tree (exclude ul\li padding area because relatedTarget, in this case, will be null) 
        isDropdownOpen = false
    }

	const handleDropdownClick = () => {
		isDropdownOpen = !isDropdownOpen // togle state on click
	}

    function darMode(): void {
        // if set via local storage previously
        if (localStorage.getItem('color-theme')) {
            if (localStorage.getItem('color-theme') === 'light') {
                document.documentElement.classList.add('dark');
                localStorage.setItem('color-theme', 'dark');
                classMode = 'dark';
            } else {
                document.documentElement.classList.remove('dark');
                localStorage.setItem('color-theme', 'light');
                classMode = 'light';
            }

        // if NOT set via local storage previously
        } else {
            if (document.documentElement.classList.contains('dark')) {
                document.documentElement.classList.remove('dark');
                localStorage.setItem('color-theme', 'light');
                classMode = 'light';
            } else {
                document.documentElement.classList.add('dark');
                localStorage.setItem('color-theme', 'dark');
                classMode = 'dark';
            }
        }
    }

    $: isDarkMode = ():boolean => {
        return classMode == 'dark';
    }

    $: getClassMode = (type: string) => {
        if(type == 'extern') {
            return classMode == 'dark' ? 'bg-surface-900' : 'bg-surface-50';
        }else if(type == 'intern') {
            return classMode == 'light' ? 'bg-surface-900 translate-x-[100%]' : 'bg-surface-50';
        }else if(type == 'icon') {
            return classMode == 'dark' ? 'fill-surface-900' : 'fill-surface-50';   
        }
    }
</script>

<div class="dropdown" on:focusout={handleDropdownFocusLoss}>
    <button class="btn hover:variant-soft-primary" on:click={handleDropdownClick}>
        <i class="fa-solid fa-palette text-lg md:!hidden"></i> 
        <span class="hidden md:inline-block">Theme</span>
        <svg class="w-6 h-6 text-gray-800 dark:text-white" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="currentColor" viewBox="0 0 24 24">
            <path fill-rule="evenodd" d="M18.4 10.3A2 2 0 0 0 17 7H7a2 2 0 0 0-1.5 3.3l4.9 5.9a2 2 0 0 0 3 0l5-6Z" clip-rule="evenodd"/>
        </svg>					  
    </button>
    <div class="card block p-4 w-60 shadow-xl fixed top-20 left-[850px]" data-popup="theme" style="opacity: 1; pointer-events: auto;" style:visibility={isDropdownOpen ? 'visible' : 'hidden'}>
        <div class="space-y-4">
        <section class="flex justify-between items-center">
            <h6 class="h6">Mode</h6>
            <div class="lightswitch-track cursor-pointer transition-all duration-[200ms] w-12 h-6 ring-[1px] ring-surface-500/30 rounded-token {getClassMode('extern')}" role="switch" aria-label="Light Switch" aria-checked="true" title="Toggle light or dark mode." tabindex="0" on:click={darMode}>
                <div class="lightswitch-thumb aspect-square scale-[0.8] flex justify-center items-center transition-all duration-[200ms] h-6 rounded-token {getClassMode('intern')} ">
                    <svg class="lightswitch-icon w-[70%] aspect-square {getClassMode('icon')}" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512">
                        <path d="M223.5 32C100 32 0 132.3 0 256S100 480 223.5 480c60.6 0 115.5-24.2 155.8-63.4c5-4.9 6.3-12.5 3.1-18.7s-10.1-9.7-17-8.5c-9.8 1.7-19.8 2.6-30.1 2.6c-96.9 0-175.5-78.8-175.5-176c0-65.8 36-123.1 89.3-153.3c6.1-3.5 9.2-10.5 7.7-17.3s-7.3-11.9-14.3-12.5c-6.3-.5-12.6-.8-19-.8z"></path>
                        <path class="{isDarkMode() ? 'hidden': ''}" d="M361.5 1.2c5 2.1 8.6 6.6 9.6 11.9L391 121l107.9 19.8c5.3 1 9.8 4.6 11.9 9.6s1.5 10.7-1.6 15.2L446.9 256l62.3 90.3c3.1 4.5 3.7 10.2 1.6 15.2s-6.6 8.6-11.9 9.6L391 391 371.1 498.9c-1 5.3-4.6 9.8-9.6 11.9s-10.7 1.5-15.2-1.6L256 446.9l-90.3 62.3c-4.5 3.1-10.2 3.7-15.2 1.6s-8.6-6.6-9.6-11.9L121 391 13.1 371.1c-5.3-1-9.8-4.6-11.9-9.6s-1.5-10.7 1.6-15.2L65.1 256 2.8 165.7c-3.1-4.5-3.7-10.2-1.6-15.2s6.6-8.6 11.9-9.6L121 121 140.9 13.1c1-5.3 4.6-9.8 9.6-11.9s10.7-1.5 15.2 1.6L256 65.1 346.3 2.8c4.5-3.1 10.2-3.7 15.2-1.6zM352 256c0 53-43 96-96 96s-96-43-96-96s43-96 96-96s96 43 96 96zm32 0c0-70.7-57.3-128-128-128s-128 57.3-128 128s57.3 128 128 128s128-57.3 128-128z"></path>
                    </svg>
                </div>
            </div>
        </section>
        <hr>
        <nav class="list-nav p-4 -m-4 max-h-64 lg:max-h-[500px] overflow-y-auto">
                <ul>
                    {#each themes as theme}
                    <li class="list-themes"><button class="option w-full h-full {getActiveTheme(theme.name)}" name="theme" value={theme.name} on:click={() => changeThemes(theme.name)}><span>{theme.icon}</span> <span class="flex-auto text-left">{theme.label}</span></button> </li>
                    {/each}
                </ul>
        </nav>
        </div>
    </div>
</div>

