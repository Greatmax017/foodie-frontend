<template>
    <dashboard-layout>
        <div slot="main-content">
            <h2 class="dash-title">Menu</h2>
                    
            <div class="page-action">
                <button class="btn btn-main" @click="$router.push('/admin/menu/add')"><span class="ti-plus"></span> Add menu item</button>
            </div>
            
            
            <section class="recent">
                <div class="">
                    <div class="activity-card">
                        <h3>Added menu item</h3>
                        
                        <div class="table-responsive">
                            <div>
                            <input type="text" v-model="searchQuery" placeholder="Search by name" class="form-control">
                            </div>
                            <table>
                                <thead>
                                    <tr>
                                        <th>ID</th>
                                        <th>Image</th>
                                        <th>Name</th>
                                        <th>Category</th>
                                        <th>price</th>
                                        <th>Available?</th>
                                        <th>Action</th>
                                    </tr>
                                </thead>
                                <tbody>
                                    <tr v-for="(menuItem, index) in filteredMenu" :key="index" >
                                        <td>#{{index + 1}}</td>
                                        <td><img :src="menuItem.image" style="border-radius: 50%" height="50px" width="50px" alt=""></td>
                                        <td>{{menuItem.name}}</td>
                                        <td>{{menuItem.category.name}}</td>
                                        <td>{{menuItem.price}}</td>
                                        <td>
                                            <span class="badge success" v-if="Number(menuItem.isAvailable) === 1">Available</span>
                                            <span class="badge warning" v-else>Not available</span>
                                        </td>
                                        <td>
                                            <button class="btn btn-main-gradient" @click="removeMenu(menuItem.id, index)"><span class="ti-trash"></span></button>
                                            <button class="btn btn-main-" @click="$router.push(`/admin/menu/update/${menuItem.id}`)"><span class="ti-pencil"></span></button>
                                        </td>
                                    </tr>
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>
            </section>
        </div>
    </dashboard-layout>
</template>

<script>
import DashboardLayout from '@/components/Layouts/DashboardLayout'

export default {
    components: {
        DashboardLayout
    },
    name: 'Menu',
    data() {
        return {
            searchQuery: '',
            menu: []
        }
    },
    computed: {
  filteredMenu() {
    if (!this.searchQuery) {
      return this.menu; // Return all menu items if search query is empty
    }

    const query = this.searchQuery.toLowerCase();
    return this.menu.filter((menuItem) =>
      menuItem.name.toLowerCase().includes(query)
      
    );
  },
},
    mounted() {
        this.getMenu()
    },
    methods: {
        getMenu() {
            this.$axios.get(`${this.$apiUrl}/menu/all`)
            .then(res => {
                this.menu = res.data.data
            })
            .catch(error => {
                console.log(error.response)
            })
        },
        removeMenu(id, index) {
            this.$axios.delete(`${this.$apiUrl}/menu/${id}/delete`, {
                headers: {
                    Authorization: `Bearer ${localStorage.authtoken}`
                }
            })
            .then(() => {
                this.$alertify.success('Menu item deleted successfully')
                this.menu.splice(index, 1)
            })
            .catch(error => {
                console.log(error.response)
            })
        }
    }
}
</script>