<template>
    <dashboard-layout>
        <div slot="main-content">
            <h2 class="dash-title">Overview</h2>
                    
            <div class="dash-cards">
                <div class="card-single">
                    <div class="card-body">
                        <span class="ti-briefcase"></span>
                        <div>
                            <h5>Sales Today</h5>
                            <h4>&#8358;{{today}}</h4>
                        </div>
                    </div>
                    <!-- <div class="card-footer">
                        <a href="">View all</a>
                    </div> -->
                </div>


                
                
                
                <div class="card-single">
                    <div class="card-body">
                        <span class="ti-reload"></span>
                        <div>
                            <h5>Pending</h5>
                            <h4>{{pending}}</h4>
                        </div>
                    </div>
                    <div class="card-footer">
                        <a @click="$router.push('/admin/orders')" href="">View all</a>
                    </div>
                </div>
                <div class="card-single">
                    <div class="card-body">
                        <span class="ti-reload"></span>
                        <div>
                            <h5>Total Sales</h5>
                            <h4>&#8358;{{total}}</h4>
                        </div>
                    </div>
                    <div class="card-footer">
                        <a @click="$router.push('/admin/orders')" href="">View all</a>
                    </div>
                </div>
                
                <!-- <div class="card-single">
                    <div class="card-body">
                        <span class="ti-check-box"></span>
                        <div>
                            <h5>Processed</h5>
                            <h4>{{proccessed}}</h4>
                        </div>
                    </div>
                    <div class="card-footer">
                        <a href="">View all</a>
                    </div>
                </div> -->
            </div>
            
            <section class="recent">
                <div class="">
                    <div class="activity-card">
                        <h3>Recent menu items</h3>
                        
                        <div class="table-responsive">
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
                                    <tr v-for="(menuItem, index) in menu" :key="index" >
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
                                            <button class="btn btn-main-gradient" @click="removeMenu(menu.id, index)"><span class="ti-trash"></span></button>
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
    name: 'AdminHome',
    data() {
        return {
            menu: [],
            total: null,
            today: null,
            pending: null,
            proccessed: null

        }
    },
    mounted() {
        this.getMenu()
        this.totalSales()
        this.todaySales()
        this.pendingOrder()
        this.proccessedOrder()
    },
    methods: {
        getMenu() {
            this.$axios.get(`${this.$apiUrl}/menu/all`)
            .then(res => {
                this.menu = res.data.data.slice(0, 3)
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
                this.$alertify.success('Menu deleted successfully')
                this.categories.splice(index, 1)
            })
            .catch(error => {
                console.log(error.response)
            })
        },
        totalSales(){
            this.$axios.get(`${this.$apiUrl}/totalsales`,
            {
                headers: {
                    Authorization: `Bearer ${localStorage.authtoken}`
                }
            })
            .then( res => {
                const result = res.data;
                this.total = result.data;
            })
            .catch(error => {
                console.log(error.response)
            }) 
        },

        // today
        todaySales(){
            this.$axios.get(`${this.$apiUrl}/todaysales`,
            {
                headers: {
                    Authorization: `Bearer ${localStorage.authtoken}`
                }
            })
            .then( res => {
                const result = res.data;
                this.today = result.data;
            })
            .catch(error => {
                console.log(error.response)
            }) 
        },
        proccessedOrder(){
            this.$axios.get(`${this.$apiUrl}/proccessed`,
            {
                headers: {
                    Authorization: `Bearer ${localStorage.authtoken}`
                }
            })
            .then( res => {
                const result = res.data;
                this.proccessed = result.data;
            })
            .catch(error => {
                console.log(error.response)
            }) 
        },
        pendingOrder(){
            this.$axios.get(`${this.$apiUrl}/pendingorders`,
            {
                headers: {
                    Authorization: `Bearer ${localStorage.authtoken}`
                }
            })
            .then( res => {
                const result = res.data;
                this.pending = result.data;
            })
            .catch(error => {
                console.log(error.response)
            }) 
        }
    }
}
</script>