<template>
    <div class="page-invoices">
        <nav class="breadcrumb" arial-label="breadcrumbs">
            <ul>
                <li><router-link to="/dashboard">Dashboard</router-link></li>
                <li class="is-active"><router-link to="/dashboard/invoices" aria-current="true">Invoices</router-link></li>
            </ul>
        </nav>
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">Invoices</h1>
            </div>

            <div class="column is-12 box">
                <table class="table is-fullwidth">
                    <thead>
                        <tr>
                            <th>#</th>
                            <th>Client</th>
                            <th>Amount</th>
                            <th>Date of Purchase</th>
                            <th>Due date</th>
                            <th>Is paid</th>
                            <th></th>
                        </tr>
                    </thead>

                    <tbody>
                        <tr
                            v-for="invoice in invoices"
                            v-bind:key="invoice.id"
                        >
                            <td>{{ invoice.invoice_number }}</td>
                            <td>{{ invoice.client_name }}</td>
                            <td>{{ invoice.gross_amount }}</td>
                            <td>{{ formatInvoiceDate(invoice.created_at) }}</td>
                            <td>{{ formatInvoiceDate(formatDueDate(invoice.get_due_date_formatted)) }}</td>
                            <td>{{ getStatusLabel(invoice) }}</td>
                            <td>
                                <router-link :to="{ name: 'Invoice', params: { id: invoice.id }}">Details</router-link>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    name: 'Invoices',
    data() {
        return {
            invoices: []
        }
    },
    mounted() {
        this.getInvoices()
    },
    methods: {
        getInvoices() {
            axios
                .get('/api/v1/invoices/')
                .then(response => {
                    for (let i = 0; i < response.data.length; i++) {
                        this.invoices.push(response.data[i])
                    }
                })
                .catch(error => {
                    console.log(JSON.stringify(error))
                })
        },
        formatInvoiceDate(dateString) {
            const originalDate = new Date(dateString);
            const options = { year: 'numeric', month: 'long', day: 'numeric' };
            return originalDate.toLocaleDateString('en-US', options);
        },
        formatDueDate(dateString) {
            const originalDate = new Date(dateString);
            
            const options = { day: '2-digit', month: '2-digit', year: 'numeric' };

            const formattedDate = originalDate.toLocaleDateString('en-US', options);

            // Manually rearrange the parts to get the desired format
            const [month, day, year] = formattedDate.split('/');
            return `${day}.${month}.${year}`;
        },
        getStatusLabel(invoice) {
            if (invoice.is_paid) {
                return "Yes"
            } else {
                return "No"
            }
        }
    }
}
</script>