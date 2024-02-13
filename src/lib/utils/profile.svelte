<script lang="ts">
	import { Avatar } from "@skeletonlabs/skeleton";

    let isDropdownOpen: boolean = false;

    const handleDropdownFocusLoss = ({ relatedTarget, currentTarget }) => {
        // use "focusout" event to ensure that we can close the dropdown when clicking outside or when we leave the dropdown with the "Tab" button
        if (relatedTarget instanceof HTMLElement && currentTarget.contains(relatedTarget)) return // check if the new focus target doesn't present in the dropdown tree (exclude ul\li padding area because relatedTarget, in this case, will be null) 
        isDropdownOpen = false
    }

	const handleDropdownClick = () => {
		isDropdownOpen = !isDropdownOpen // togle state on click
	}
</script>


<div class="flex items-center md:order-2 space-x-3 md:space-x-0 rtl:space-x-reverse" on:focusout={handleDropdownFocusLoss}>
    <button type="button" class="flex text-sm bg-gray-800 rounded-full md:me-0 focus:ring-4 focus:ring-gray-300 dark:focus:ring-gray-600" id="user-menu-button" aria-expanded="false" data-dropdown-toggle="user-dropdown" data-dropdown-placement="bottom" on:click={handleDropdownClick}>
      <span class="sr-only">Open user menu</span>
      <Avatar initials="JD" background="bg-primary-500" class="h9 w-9" />
    </button>
    <!-- Dropdown menu -->
    <div class="z-50 block my-4 text-base list-none bg-white divide-y divide-gray-100 rounded-lg shadow dark:bg-gray-700 dark:divide-gray-600 fixed top-14 right-1" id="user-dropdown" style:visibility={isDropdownOpen ? 'visible' : 'hidden'}>
      <div class="px-4 py-3">
        <span class="block text-sm text-gray-900 dark:text-white">Bonnie Green</span>
        <span class="block text-sm  text-gray-500 truncate dark:text-gray-400">name@flowbite.com</span>
      </div>
      <ul class="py-2" aria-labelledby="user-menu-button">
        <li>
          <a class="block px-4 py-2 text-sm text-gray-700 hover:bg-gray-100 dark:hover:bg-gray-600 dark:text-gray-200 dark:hover:text-white">Dashboard</a>
        </li>
        <li>
          <a class="block px-4 py-2 text-sm text-gray-700 hover:bg-gray-100 dark:hover:bg-gray-600 dark:text-gray-200 dark:hover:text-white">Settings</a>
        </li>
        <li>
          <a class="block px-4 py-2 text-sm text-gray-700 hover:bg-gray-100 dark:hover:bg-gray-600 dark:text-gray-200 dark:hover:text-white">Earnings</a>
        </li>
        <li>
          <a class="block px-4 py-2 text-sm text-gray-700 hover:bg-gray-100 dark:hover:bg-gray-600 dark:text-gray-200 dark:hover:text-white">Sign out</a>
        </li>
      </ul>
    </div>
</div>