<template>
    <dashboard-layout>
        <div slot="main-content">
            <h2 class="dash-title">Order Details </h2>
            
            <section class="recent">
                <div class="">
                    <div class="activity-card">
                        <div class="customer-info">
                            <h4>Customer Information</h4>
                            <p><strong>Name:</strong> {{ orders.email }}   </p>
                            <p><strong>Phone:</strong>  {{ orders.phone }}</p>
                            <p><strong>Address:</strong>  {{ orders.address }}</p>
                            <p><strong>Location:</strong>  {{ orders.location }}</p>
                        </div>
                       
                        <div class="order-details">
                            <h4>Order Details</h4>               
                            <table>
                                <thead>
                                    <tr>
                                        <th>Quantity</th>
                                        <th>Food Item</th>
                                        <th>Price</th>
                                    </tr>
                                </thead>
                                <tbody v-for="(item, index) in orders.items" :key="index">
                                    <tr>
                                        <td>{{ item.quantity }}</td>
                                        <td>{{ item.menu.name }}</td>
                                        <td>&#8358;{{ item.price * item.quantity }}</td>
                                    </tr>
                                    
                                    <tr v-for="(protein, pIndex) in item.proteins" :key="pIndex">
                                        <td>{{ protein.quantity }}</td>
                                        <td>{{ protein.details.name }}</td>
                                        <td>&#8358;{{ protein.price * protein.quantity }}</td>
                                    </tr>
                                </tbody>
                                <tbody>
                                    <tr>
                                        <td></td>
                                        <td></td>
                                        <td>Delivery: &#8358;{{ orders.deliveryFee }}</td>
                                    </tr>
                                    <tr>
                                        <td></td>
                                        <td></td>
                                        <td>Takeaway: &#8358;250</td>
                                    </tr>
                                    <tr>
                                        <td></td>
                                        <td></td>
                                        <td><strong>Total: &#8358;{{ orders.amount }}</strong></td>
                                    </tr>
                                </tbody>
                            </table>
                        </div>

                        <div class="radio-group">
                            <label>
                                <input type="radio" name="status" value="processing" v-model="orderState" @change="handleRadioChange" />
                                Processing
                            </label>
                            <label>
                                <input type="radio" name="status" value="completed" v-model="orderState" @change="handleRadioChange"/>
                                Completed
                            </label>
                        </div>

                        <div class="receipt-section">
                            <button class="btn btn-main" @click="$router.push(`/admin/receipt/${orderId}`)">Print Receipt</button>
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
    name: 'Orders',
    data() {
        return {
            orders: [],
            orderId: null,
            orderState: ""
        }
    },
    methods: {
        getOrder() {
            this.$axios.get(`${this.$apiUrl}/orders/${this.orderId}`, {
                headers: {
                    Authorization: `Bearer ${localStorage.authtoken}`
                }
            })
            .then(res => {
                this.orders = res.data.data;
                this.orderState = this.orders.state;
            })
            .catch(error => {
                console.log(error.response)
            })
        },
        handleRadioChange() {
            console.log('Selected order state:', this.orderState);

            this.$axios.post(`${this.$apiUrl}/order/state`, {id: this.orderId, state: this.orderState}, {
                headers: {
                    Authorization: `Bearer ${localStorage.authtoken}`
                }
            })
        },
        printReceipt() {
            // Implement the logic for printing the receipt
        },
    },
    mounted() {
        const id = this.$route.params.id;
        this.orderId = id;
        this.getOrder()
    },
}
</script>

<style>
.recent {
  background-color: #f8f8f8;
  padding: 20px;
}

.activity-card {
  background-color: #fff;
  border-radius: 5px;
  padding: 20px;
}

.customer-info,
.order-details,
.status-section,
.receipt-section {
  margin-bottom: 20px;
}

h4 {
  font-size: 18px;
  margin-bottom: 10px;
}

table {
  width: 100%;
  border-collapse: collapse;
}

th,
td {
  padding: 8px;
  text-align: left;
  border-bottom: 1px solid #ddd;
}

.status-section select {
  width: 200px;
  padding: 8px;
  border-radius: 5px;
  border: 1px solid #000000;
}

.receipt-section {
  display: flex;
  justify-content: flex-end;
}

.receipt-section button {
  padding: 10px 20px;
  background-color: #4caf50;
  color: #fff;
  border: none;
  border-radius: 5px;
  font-weight: bold;
  cursor: pointer;
}

.receipt-section button:hover {
  background-color: #45a049;
}





</style>