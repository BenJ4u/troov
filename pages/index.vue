<template>
    <BaseNavbar />
    <section class="flex h-screen justify-center items-center bg-gray-200/50 dark:bg-secondary z-0">
        <div class="m-1">
            <div class="relative overflow-x-auto">
                <table v-if="items.length > 0" class="w-full text-sm text-left text-gray-500 dark:text-gray-400">
                    <thead class="text-xs text-gray-700 uppercase bg-gray-50 dark:bg-gray-700 dark:text-gray-400">
                        <tr>
                            <th scope="col" class="px-6 py-3">
                                Nom de l'objet
                            </th>
                            <th scope="col" class="px-6 py-3">
                                Ajouté le
                            </th>
                            <th v-if="authStore.isLoggedIn" scope="col" class="px-6 py-3">
                                Actions
                            </th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="item in items" class="bg-white border-b dark:bg-gray-800 dark:border-gray-700">
                            <th scope="row" class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap dark:text-white">
                                {{ item.name }}
                            </th>
                            <th scope="row" class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap dark:text-white">
                                {{ DateTime.fromISO(item.createdAt).toLocaleString(DateTime.DATE_FULL) }}
                            </th>
                            <td v-if="authStore.isLoggedIn" class="text-center px-6 py-4 space-x-1">
                                <NuxtLink :to="`/items/edit/${item._id}`">
                                    <button :disabled="authStore.isLoggedIn ? false : true" class="fill-primary">
                                        <svg width="20" height="20" viewBox="0 0 27 27"
                                            xmlns="http://www.w3.org/2000/svg">
                                            <path
                                                d="M21.9375 13.5C21.9375 13.2762 22.0264 13.0616 22.1846 12.9034C22.3429 12.7451 22.5575 12.6562 22.7812 12.6562C23.005 12.6562 23.2196 12.7451 23.3779 12.9034C23.5361 13.0616 23.625 13.2762 23.625 13.5V22.7812C23.625 23.005 23.5361 23.2196 23.3779 23.3779C23.2196 23.5361 23.005 23.625 22.7812 23.625H4.21875C3.99497 23.625 3.78036 23.5361 3.62213 23.3779C3.46389 23.2196 3.375 23.005 3.375 22.7812V4.21875C3.375 3.99497 3.46389 3.78036 3.62213 3.62213C3.78036 3.46389 3.99497 3.375 4.21875 3.375H13.5C13.7238 3.375 13.9384 3.46389 14.0966 3.62213C14.2549 3.78036 14.3438 3.99497 14.3438 4.21875C14.3438 4.44253 14.2549 4.65714 14.0966 4.81537C13.9384 4.97361 13.7238 5.0625 13.5 5.0625H5.0625V21.9375H21.9375V13.5Z" />
                                            <path
                                                d="M12.3915 14.6137L13.7837 14.4146L22.336 5.86406C22.4166 5.78622 22.4808 5.69312 22.5251 5.59018C22.5693 5.48724 22.5925 5.37652 22.5935 5.26449C22.5945 5.15246 22.5731 5.04135 22.5307 4.93766C22.4883 4.83396 22.4256 4.73976 22.3464 4.66054C22.2672 4.58131 22.173 4.51866 22.0693 4.47624C21.9656 4.43381 21.8545 4.41247 21.7425 4.41344C21.6304 4.41441 21.5197 4.43769 21.4168 4.48191C21.3138 4.52613 21.2207 4.59041 21.1429 4.67099L12.589 13.2216L12.3898 14.6137H12.3915ZM23.529 3.47624C23.7643 3.71131 23.9508 3.99043 24.0782 4.29764C24.2055 4.60485 24.271 4.93414 24.271 5.26668C24.271 5.59923 24.2055 5.92851 24.0782 6.23572C23.9508 6.54294 23.7643 6.82205 23.529 7.05712L14.7793 15.8068C14.6503 15.9363 14.4827 16.0204 14.3018 16.0464L11.5174 16.4447C11.3876 16.4633 11.2553 16.4515 11.1309 16.4101C11.0066 16.3686 10.8935 16.2988 10.8008 16.2061C10.7081 16.1134 10.6383 16.0004 10.5969 15.876C10.5555 15.7516 10.5436 15.6193 10.5623 15.4896L10.9605 12.7052C10.9861 12.5244 11.0696 12.3569 11.1985 12.2276L19.9498 3.47793C20.4245 3.0034 21.0682 2.73682 21.7394 2.73682C22.4106 2.73682 23.0544 3.0034 23.529 3.47793V3.47624Z" />
                                        </svg>
                                    </button>
                                </NuxtLink>

                                <button :disabled="authStore.isLoggedIn ? false : true" v-on:click="deleteItem(item._id)" class="fill-red-400">
                                    <svg width="20" height="20" viewBox="0 0 27 27" xmlns="http://www.w3.org/2000/svg">
                                        <path fill-rule="evenodd" clip-rule="evenodd"
                                            d="M7.98752 5.625H3.375C2.75368 5.625 2.25 6.12868 2.25 6.75C2.25 7.37132 2.75368 7.875 3.375 7.875H4.56812L5.49533 22.7105C5.6065 24.4892 7.08155 25.875 8.86376 25.875H18.1362C19.9185 25.875 21.3934 24.4892 21.5047 22.7105L22.4319 7.875H23.625C24.2463 7.875 24.75 7.37132 24.75 6.75C24.75 6.12868 24.2463 5.625 23.625 5.625H21.3832C21.377 5.62494 21.3708 5.62494 21.3646 5.625H19.0125C18.4913 3.05748 16.2214 1.125 13.5 1.125C10.7787 1.125 8.5087 3.05748 7.98752 5.625ZM10.317 5.625H16.683C16.2197 4.31416 14.9695 3.375 13.5 3.375C12.0305 3.375 10.7804 4.31416 10.317 5.625ZM20.1774 7.875H6.82251L7.74096 22.5702C7.77801 23.1631 8.26969 23.625 8.86376 23.625H18.1362C18.7303 23.625 19.222 23.1631 19.2591 22.5702L20.1774 7.875ZM16.0639 11.2609C16.6837 11.3043 17.1511 11.8419 17.1076 12.4617L16.6368 19.1952C16.5935 19.8151 16.0559 20.2824 15.4361 20.2391C14.8162 20.1957 14.3489 19.6581 14.3923 19.0383L14.8632 12.3048C14.9065 11.6849 15.4441 11.2176 16.0639 11.2609ZM10.9361 11.261C11.5559 11.2177 12.0935 11.6849 12.1368 12.3048L12.6078 19.0384C12.6511 19.6581 12.1837 20.1958 11.564 20.2391C10.9441 20.2824 10.4065 19.8151 10.3632 19.1953L9.89234 12.4617C9.849 11.842 10.3163 11.3043 10.9361 11.261Z" />
                                    </svg>
                                </button>
                            </td>
                        </tr>
                    </tbody>
                </table>

                <p class="text-secondary dark:text-white" v-else>Il n'y a aucun objet à afficher.</p>
            </div>
        </div>
    </section>
</template>

<script setup lang="ts">
import { useAuthStore } from '~/store/auth';
import { DateTime } from 'luxon';

const config = useRuntimeConfig();
const auth = useCookie('auth').value as any;
const authStore = useAuthStore();

/**
 * @description Permet de récupérer la liste des items de façon asynchrone
 */
const { data: items, refresh: refreshItems }: any = await useAsyncData(
    'items',
    () => $fetch(`${config.public.API_BASE_URL}/item`, {
        method: 'GET',
    })
);

/**
 * @description Permet de supprimer un item et de recharger la liste des items
 * @param {string} id - id unique de l'item
 */
const deleteItem = (id: any) => {
    $fetch(`${config.public.API_BASE_URL}/item/${id}`, {
        method: 'DELETE',
        headers: {
            "Authorization": "Bearer " + auth?.token
        },
    }).then((item: any) => {

        delete items.value[item];
        refreshItems();
    });
}
</script>