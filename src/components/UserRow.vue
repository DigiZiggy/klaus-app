<template> 
    <div class="grid grid-cols-2 py-2.5 mb-1 items-center transition duration-300 ease-in-out border-4 border-transparent hover:bg-gray-100 rounded cursor-pointer"
         :class="{ 'border-l-indigo-600 bg-gray-100': isChecked }">
        <div class="text-sm pl-3">
            <div class="flex items-center">
                <div class="mr-3">
                    <div class="border border-gray-300 rounded-sm w-4 h-4 flex flex-shrink-0 justify-center items-center relative">
                        <input :checked="isChecked" @click="onChange()" placeholder="checkbox" type="checkbox" class="checkbox opacity-0 checked:opacity-100 checked:text-indigo-600 accent-indigo-600 cursor-pointer w-full h-full" />
                        <div class="check-icon hidden bg-indigo-600 text-white rounded-sm">
                            <svg class="icon icon-tabler icon-tabler-check" xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
                                <path stroke="none" d="M0 0h24v24H0z"></path>
                                <path d="M5 12l5 5l10 -10"></path>
                            </svg>
                        </div>
                    </div>
                </div>
                <div class="flex-shrink-0 w-8 h-8">
                    <img class="w-full h-full rounded-full"
                        :src="avatarPicture"
                        :alt="user.name"
                        @error="onImgError()"/>
                </div>
                <div class="ml-3 text-sm">
                    <p class="text-sm text-gray-800 font-medium whitespace-pre">
                        {{`${user.name}`}}
                    </p>
                    <p class="text-xs text-gray-500 font-normal whitespace-no-wrap">
                        {{user.email}}
                    </p>
                </div>
            </div>
        </div>
    
        <div class="flex items-center justify-between">
            <div class="py-2 px-2 text-sm capitalise text-center focus:outline-none leading-none rounded"
                 :class="getUserRoleClass">
                {{userRole}}
            </div>
            <div class="flex items-center text-center text-sm pr-6">
                <button class="inline-flex items-center mr-2 h-8 px-2 py-1.5 border border-gray-300 text-gray-400 shadow-sm shadow-gray-300 text-sm font-medium rounded-md hover:bg-gray-200 hover:border-indigo-700 hover:text-indigo-700 hover:bg-indigo-100">
                    <svg xmlns="http://www.w3.org/2000/svg" class="mr-1" width="15" height="15" viewBox="0 0 15 15" fill="none" fill-rule="evenodd" stroke="currentColor">
                        <path d="M11.8536 1.14645C11.6583 0.951184 11.3417 0.951184 11.1465 1.14645L3.71455 8.57836C3.62459 8.66832 3.55263 8.77461 3.50251 8.89155L2.04044 12.303C1.9599 12.491 2.00189 12.709 2.14646 12.8536C2.29103 12.9981 2.50905 13.0401 2.69697 12.9596L6.10847 11.4975C6.2254 11.4474 6.3317 11.3754 6.42166 11.2855L13.8536 3.85355C14.0488 3.65829 14.0488 3.34171 13.8536 3.14645L11.8536 1.14645ZM4.42166 9.28547L11.5 2.20711L12.7929 3.5L5.71455 10.5784L4.21924 11.2192L3.78081 10.7808L4.42166 9.28547Z" />
                    </svg>
                    <span class="text-gray-600 hover:text-indigo-700">Edit</span>
                </button>
                <button @click="onDelete()" class="inline-flex items-center h-8 w-8 px-1.5 py-1.5 border border-gray-300 text-gray-400 shadow-sm shadow-gray-300 text-sm font-medium rounded-md hover:text-red-700 hover:border-red-700 hover:bg-red-100">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" />
                    </svg>
                </button>
            </div>
        </div>
    </div>   
</template>

<script>
export default {
    name:'UserRow',
    props: {
        user: {
            type: Object,
            require: true
        },
        method: {
            type: Function
        }
    },
    data: function () {
        return {
            imgError: false,
            isChecked: !!this.user.checked,
            isAccountManager: this.user.role === "ACCOUNT_MANAGER",
            isAdmin: this.user.role === "ADMIN",
            isAgent: this.user.role === "AGENT",
            isExternalReviewer: this.user.role === "EXTERNAL_REVIEWER",
        }
    },
    computed: {
        avatarPicture() {
            return (this.imgError) ? require("@/assets/images/klaus.png") : this.user.avatar;
        },
        getUserRoleClass() {
            if (this.isAccountManager) {
                return "text-red-700 bg-red-100";
            }
            if (this.isAdmin) {
                return "text-violet-700 bg-violet-100";
            }
            if (this.isAgent) {
                return "text-blue-700 bg-blue-100";
            }
            if (this.isExternalReviewer) {
                return "text-orange-700 bg-orange-100";
            }
            return "";
        },
        userRole() {
            var role = this.user.role.replace('_', ' ').toLowerCase();
            return role.charAt(0).toUpperCase() + role.slice(1);
        }
    },
    methods: {
		onImgError() {
			this.imgError = true;
		},
        onChange() {
            this.$emit('on-change', this.user.id);
        },
        onDelete() {
            this.$emit('on-delete', this.user.id);
        }
    },
}
</script>
