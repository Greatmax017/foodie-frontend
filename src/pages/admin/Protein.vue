<template>
    <dashboard-layout>
        <div slot="main-content">
            <h2 class="dash-title">Protein</h2>
                    
            <div class="page-action">
                <button class="btn btn-main" @click="$router.push('/admin/protein/add')"><span class="ti-plus"></span> Add Protein</button>
            </div>
            
            
            <section class="recent">
                <div class="">
                    <div class="activity-card">
                        <h3>Added Protein</h3>
                        
                        <div class="table-responsive">
                            <table>
                                <thead>
                                    <tr>
                                        <th>ID</th>
                                        <th>Name</th>
                                        <th>Price</th>
                                        <th>Action</th>
                                    </tr>
                                </thead>
                                <tbody>
                                    <tr v-for="(category, index) in categories" :key="index" >
                                        <td>#{{index + 1}}</td>
                                        <td>{{category.name}}</td>
                                        <td>{{category.price}}</td>
                                        <td>
                                            <button class="btn btn-main-gradient" @click="removeCategory(category.id, index)"><span class="ti-trash"></span></button>
                                            
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
    name: 'Categories',
    components: {
        DashboardLayout,
    },
    data() {
        return {
            categories: [],
        }
    },
    mounted() {
        this.getCategories()
    },
    methods: {
        getCategories() {
            this.$axios.get(`${this.$apiUrl}/menu/protein`)
            .then(res => {
                this.categories = res.data.data
            })
            .catch(error => {
                console.log(error.response)
            })
        },
        removeCategory(id, index) {
            this.$axios.delete(`${this.$apiUrl}/protein/delete/${id}`, {
                headers: {
                    Authorization: `Bearer ${localStorage.authtoken}`
                }
            })
            .then(() => {
                this.$alertify.success('Protein deleted successfully')
                this.categories.splice(index, 1)
            })
            .catch(error => {
                console.log(error.response)
            })
        }
    }
}
</script>