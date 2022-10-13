<template>
    <div class="bg-gray-200 min-h-screen h-fit">
        <div class="container mx-auto py-9">
            <div class="flex items-center justify-between">
                <div>Account users</div>
                <div class="relative">
                    <div class="flex absolute inset-y-0 left-0 items-center px-2 pointer-events-none">
                        <svg aria-hidden="true" class="w-4 h-4 text-gray-500 dark:text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"></path></svg>
                    </div>
                    <input v-model="search" type="text" class="border border-gray-300 text-sm transition duration-200 ease-in-out bg-white h-10 px-8 rounded z-0 focus:bg-gray-100 focus:outline-none search-input" placeholder="Search">
                    <button type="submit" class="ml-4 text-white right-2.5 bottom-2.5 bg-indigo-700 hover:bg-indigo-800 focus:outline-none font-medium rounded text-sm px-4 py-2.5 dark:bg-indigo-600 dark:hover:bg-indigo-700">Connect users</button>
                </div>
            </div>
            <div class="py-8 md:py-4">
                <div class="my-2 flex sm:flex-row flex-col"></div>
                <div class="-mx-4 sm:-mx-8 px-4 sm:px-8 py-6 overflow-x-auto ">
                    <div class="bg-white inline-block min-w-full shadow md:shadow-xl md:p-4 rounded-lg overflow-hidden ">
                    
                        <div class="flex justify-start items-center mt-2 mb-7">
                            <div class="ml-4 text-gray-800">{{usersAmount}} users selected</div>

                            <div class="flex items-center text-center text-sm ml-6">
                                <button class="inline-flex items-center mr-2 h-8 px-2 py-1.5 border border-gray-300 text-gray-400 shadow-sm shadow-gray-300 text-sm font-medium rounded-md hover:bg-gray-200 hover:border-indigo-700 hover:text-indigo-700 hover:bg-indigo-100">
                                    <svg xmlns="http://www.w3.org/2000/svg" class="mr-1" width="15" height="15" viewBox="0 0 15 15" fill="none" fill-rule="evenodd" stroke="currentColor">
                                        <path d="M11.8536 1.14645C11.6583 0.951184 11.3417 0.951184 11.1465 1.14645L3.71455 8.57836C3.62459 8.66832 3.55263 8.77461 3.50251 8.89155L2.04044 12.303C1.9599 12.491 2.00189 12.709 2.14646 12.8536C2.29103 12.9981 2.50905 13.0401 2.69697 12.9596L6.10847 11.4975C6.2254 11.4474 6.3317 11.3754 6.42166 11.2855L13.8536 3.85355C14.0488 3.65829 14.0488 3.34171 13.8536 3.14645L11.8536 1.14645ZM4.42166 9.28547L11.5 2.20711L12.7929 3.5L5.71455 10.5784L4.21924 11.2192L3.78081 10.7808L4.42166 9.28547Z" />
                                    </svg>
                                    <span class="text-gray-600 hover:text-indigo-700">Edit</span>
                                </button>
                                <button @click="deleteAllSelected" class="inline-flex items-center h-8 px-2 py-1.5 border border-gray-300 text-gray-400 shadow-sm shadow-gray-300 text-sm font-medium rounded-md hover:text-red-700 hover:border-red-700 hover:bg-red-100">
                                    <svg xmlns="http://www.w3.org/2000/svg" class="mr-1 h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" />
                                    </svg>
                                    <span class="text-gray-600 hover:text-red-700">Delete</span>
                                </button>
                            </div>              
                        </div>

                        <div class="grid grid-cols-2 items-center mt-2 mb-5">
                            <div class="flex justify-start pl-4">
                                <div class="mr-3 border border-gray-300 rounded-sm w-4 h-4 flex flex-shrink-0 justify-center items-center relative">
                                    <input :checked="isAllUsersChecked" @click="onAllUsersSelect()" placeholder="checkbox" type="checkbox" class="checkbox opacity-0 checked:opacity-100 checked:text-indigo-600 accent-indigo-600 cursor-pointer w-full h-full" />
                                    <div class="check-icon hidden bg-indigo-600 text-white rounded-sm">
                                        <svg class="icon icon-tabler icon-tabler-check" xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
                                            <path stroke="none" d="M0 0h24v24H0z"></path>
                                            <path d="M5 12l5 5l10 -10"></path>
                                        </svg>
                                    </div>
                                </div>
                                <span class="text-sm text-gray-500">User</span>
                            </div>
                            <div class="flex hover:cursor-pointer" @click="sortByRole()">
                                <span class="text-sm text-gray-500">Permission</span>
                                <svg xmlns="http://www.w3.org/2000/svg" class="bi h-5 w-5" fill="currentColor" viewBox="0 0 16 16">
                                    <path fill-rule="evenodd" :d="sortDirectionIconPath" />
                                </svg>
                            </div>
                        </div>

                        <user-row v-for="user in filteredUserList" :key="`${user.id}-${user.checked}`" :user="user" @on-change="onUserSelect" @on-delete="onUserDelete"></user-row>

                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
  
<script>
import json from "@/assets/json/users.json";
import UserRow from './UserRow.vue'
export default {
    name: "UserList",
    components:{
        UserRow,
    },
    data: function() {
        return {
        search: '',
        users: json.users,
        sortDirection: 'asc'
        };
    },
    computed: {
        filteredUserList() {
            var users = this.users.filter(user => {
                return user.name.toLowerCase().includes(this.search.toLowerCase())
            });
            users.sort((a,b) => {
                    let modifier = 1;
                    if (this.sortDirection === 'desc') {
                        modifier = -1;
                    }
                    if (a.role < b.role) {
                        return -1 * modifier;
                    }
                    if (a.role > b.role) {
                        return 1 * modifier;
                    }
                    return 0;
                });
            return users;
        },
        isAllUsersChecked() {
            return this.users.every(user => user.checked);
        },
        usersAmount() {
            return this.filteredUserList.filter(user => user.checked).length > 0 ? this.filteredUserList.filter(user => user.checked).length : "No";
        },
        sortDirectionIconPath() {
            return this.sortDirection === 'asc' ? 'M8 4a.5.5 0 0 1 .5.5v5.793l2.146-2.147a.5.5 0 0 1 .708.708l-3 3a.5.5 0 0 1-.708 0l-3-3a.5.5 0 1 1 .708-.708L7.5 10.293V4.5A.5.5 0 0 1 8 4z' :
            'M8 12a.5.5 0 0 0 .5-.5V5.707l2.146 2.147a.5.5 0 0 0 .708-.708l-3-3a.5.5 0 0 0-.708 0l-3 3a.5.5 0 1 0 .708.708L7.5 5.707V11.5a.5.5 0 0 0 .5.5z';
        },
    },
    methods: {
        deleteAllSelected() {
            this.users = this.users.filter(user => !user.checked);

            return this.users;  
        },
        onUserDelete(userId) {
            const index = this.users.findIndex((user) => user.id === userId);
            this.users.splice(index, 1);

            return this.users;  
        },
        onAllUsersSelect() {
            var isChecked = this.isAllUsersChecked;
            this.users.map(user => user.checked = !isChecked);  
        },
        onUserSelect(userId) {
            this.users.map(user => {
                if (user.id === userId && user.checked === undefined) {
                    user.checked = true;
                } else if (user.id === userId) {
                    user.checked = !user.checked;
                }
            });    
        },
        sortByRole() {
            this.sortDirection = this.sortDirection === 'asc' ? 'desc' : 'asc';
        }
    },
};
</script>
