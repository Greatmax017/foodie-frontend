<template>
<!DOCTYPE html>
<html>
<head>
    <title>Receipt</title>
  
</head>
<body>
    <div class="header">
        <h2>Cakes and Pastries</h2>
        <p>Futa South Gate</p>
        <hr />
        <h5>Customer Information</h5>
                <p><strong>Name:</strong> {{ orders.name }}   </p>
                <p><strong>Phone:</strong>  {{ orders.phone }}</p>
                <p><strong>Address:</strong>  {{ orders.address }}</p>
                <p><strong>Location:</strong>  {{ orders.location }}</p>
    </div>

    <table class="table-container">
        <thead>
            <tr>
                <th>Item</th>
                <th>Quantity</th>
                <th>Price</th>
            </tr>
        </thead>
        <tbody v-for="(food, index) in JSON.parse(orders.items)" :key="index">
            <tr>
                <td>{{ food.name }}</td>
                <td>{{ food.quantity }}</td>
                <td> &#8358;{{ food.price * food.quantity }}</td>
            </tr>
            <tr v-for="(protein, pIndex) in food.protein" :key="pIndex">
                <td>{{ protein.name }}</td>
                <td>{{ protein.quantity }}</td>
                <td>&#8358;{{ protein.price }}</td>
            </tr>
            </tbody>
            <tbody>
            <tr>
                <td></td>
                <td>Delivery</td>
                <td> &#8358;{{orders.deliveryFee}}</td>
            </tr>
            <tr>
                <td></td>
                <td>Takeaway</td>
                <td> &#8358;210</td>
            </tr>
           
            <tr class="total-row">
                <td></td>
                <td>Total:</td>
                
                <td>&#8358;{{ orders.amount }}</td>
            </tr>
        </tbody>
    </table>

    <div class="thank-you">
        <p>Thank you for your purchase!</p>
    </div>
     <!-- Print button section -->
     <div class="print-button">
        <button class="print-btn" onclick="window.print()">Print Receipt</button>
    </div>
</body>
</html>
</template>
<script>


export default {
    components: {
        
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

        

        
    },
    mounted() {
        
        const id = this.$route.params.id;
        this.orderId = id;
        this.getOrder()
    },
}
</script>
<style>
body {
    font-family: Arial, sans-serif;
}
.header {
    text-align: center;
    margin-bottom: 10px;
}
.table-container {
    margin: 0 auto;
    border-collapse: collapse;
    width: 80%;
}
.table-container th, .table-container td {
    border: 1px solid #000;
    padding: 8px;
    text-align: left;
}
.table-container th {
    background-color: #f2f2f2;
}
.total-row {
    font-weight: bold;
}
.thank-you {
    text-align: center;
    margin-top: 20px;
}
.print-button {
            text-align: center;
            margin-top: 20px;
        }
        .print-btn {
            padding: 10px 20px;
            background-color: #007BFF;
            color: #fff;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
</style>
