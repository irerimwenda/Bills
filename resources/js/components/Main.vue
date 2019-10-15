<template>
    <div class="container">

        <div class="row justify-content-center">
            <div class="col-md-10 mt-3">

                <h3 class="text-center">Monthly Bills</h3>
                
              <div class="card-body table-responsive p-0">
                <table class="table table-bordered table-hover mt-4">
                <thead>
                    <tr>
                    <th scope="col">#</th>
                    <th scope="col">Month</th>
                    <th scope="col">Total Litres</th>
                    <th scope="col">Total Amount Billed</th>
                    <th scope="col">Total Payments</th>
                    <th scope="col">Mpesa/Cash</th>
                    <th scope="col">Direct Deposit</th>                    
                    <th scope="col">Balance Pending</th>
                    <th scope="col">Unpaid Balance</th>
                    <th scope="col">Actions</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="bill in bills" :key="bill.id">
                    <th scope="row">{{bill.id}}</th>
                    <td>{{bill.month}}</td>
                    <td>{{bill.totallitres}}</td>
                    <td>{{bill.totalamountbilled}}</td>
                    <td>{{bill.totalpayments}}</td>
                    <td>{{bill.mpesa}}</td>
                    <td>{{bill.directdeposit}}</td>
                    <td>{{bill.balancepending}}</td>
                    <td>{{bill.unpaidbalance}}</td>
                    <td>
                        <a @click="editModal(bill)">
                            <i class="fa fa-edit" style="color:#3490dc"></i>
                        </a>
                            
                        <a @click="deleteBill(bill.id)" class="pl-3">
                            <i class="fa fa-trash" style="color:#e3342f"></i>
                        </a>
                    </td>

                    </tr>
                </tbody>
                </table>
              </div>

                <button class="btn btn-outline-primary btn-sm" @click="newModal">+ Add Bill</button>
            </div>
        </div>

        <!-- Modal -->
        <div class="modal fade" id="addNewCenter" tabindex="-1" role="dialog" aria-labelledby="addNewCenterTitle" aria-hidden="true">
        <div class="modal-dialog modal-dialog-centered" role="document">
            <div class="modal-content">
            <div class="modal-header">
                <h5 v-show="!editmode" class="modal-title" id="addNewCenterTitle">Add New Bill</h5>
                <h5 v-show="editmode" class="modal-title" id="addNewCenterTitle">Update Monthly Bill</h5>
                <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                <span aria-hidden="true">&times;</span>
                </button>
            </div>

            <form @submit.prevent="editmode ? updateBill() : createBill()">
                <div class="modal-body">

                    <div class="form-group">
                        <label>Month</label>
                        <select name="month" v-model="form.month" id="month" class="form-control" :class="{
                            'is-invalid': form.errors.has('month') }">
                            <option value="" disabled>-Select Month-</option>
                            <option value="January">January</option>
                            <option value="February">February</option>
                            <option value="March">March</option>
                            <option value="April">April</option>
                            <option value="May">May</option>
                            <option value="June">June</option>
                            <option value="July">July</option>
                            <option value="August">August</option>
                            <option value="September">September</option>
                            <option value="October">October</option>
                            <option value="November">November</option>
                            <option value="December">December</option>
                        </select>
                        <has-error :form="form" field="type"></has-error>
                    </div>

                    <div class="form-group">
                        <label>Total Litres</label>
                        <input v-model="form.totallitres" type="text" name="totallitres" id="totallitres"
                            class="form-control" :class="{ 'is-invalid': form.errors.has('totallitres') }">
                        <has-error :form="form" field="totallitres"></has-error>
                    </div>

                    <div class="form-group">
                        <label>Total Amount Billed</label>
                        <input v-model="form.totalamountbilled" type="text" name="totalamountbilled" id="totalamountbilled"
                            class="form-control" :class="{ 'is-invalid': form.errors.has('totalamountbilled') }">
                        <has-error :form="form" field="totalamountbilled"></has-error>
                    </div>

                    <div class="form-group">
                        <label>Total Payments</label>
                        <input v-model="form.totalpayments" type="text" name="totalpayments" id="totalpayments"
                            class="form-control" :class="{ 'is-invalid': form.errors.has('totalpayments') }">
                        <has-error :form="form" field="totalpayments"></has-error>
                    </div>

                    <div class="form-group">
                        <label>Mpesa/Cash</label>
                        <input v-model="form.mpesa" type="text" name="mpesa" id="mpesa"
                            class="form-control" :class="{ 'is-invalid': form.errors.has('mpesa') }">
                        <has-error :form="form" field="mpesa"></has-error>
                    </div>

                    <div class="form-group">
                        <label>Direct Deposit</label>
                        <input v-model="form.directdeposit" type="text" name="directdeposit" id="directdeposit"
                            class="form-control" :class="{ 'is-invalid': form.errors.has('directdeposit') }">
                        <has-error :form="form" field="directdeposit"></has-error>
                    </div>

                    <div class="form-group">
                        <label>Balance Pending</label>
                        <input v-model="form.balancepending" type="text" name="balancepending" id="balancepending"
                            class="form-control" :class="{ 'is-invalid': form.errors.has('balancepending') }">
                        <has-error :form="form" field="balancepending"></has-error>
                    </div>

                    <div class="form-group">
                        <label>Unpaid Balance</label>
                        <input v-model="form.unpaidbalance" type="text" name="unpaidbalance" id="unpaidbalance"
                            class="form-control" :class="{ 'is-invalid': form.errors.has('unpaidbalance') }">
                        <has-error :form="form" field="unpaidbalance"></has-error>
                    </div>

                </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                <button v-show="editmode" type="submit" class="btn btn-success">Update</button>
                <button v-show="!editmode" type="submit" class="btn btn-primary">Create</button>
            </div>

        </form>

        </div> 
        </div>
        </div>

        </div>
</template>

<script>
    export default {
        mounted() {
            console.log('Component mounted.')
        },
        created() {
           this.getBills();
            Fire.$on('afterCreated', () => {
                this.getBills();
            });
        },
        data() {
            return {
                editmode: false,
                bills: {},
                form: new Form({
                    id: '',
                    month: '',
                    totallitres: '',
                    totalamountbilled: '',
                    totalpayments: '',
                    mpesa: '',
                    directdeposit: '',
                    balancepending: '',
                    unpaidbalance: '',
                })
            }
        },
        methods: {
            getBills() {
                axios.get('http://5da4468aa6593f001407a4ce.mockapi.io/api/trial/Bills')
                .then(response => {
                    this.bills = response.data;
                })
                .catch();
            },
            createBill() {
                this.$Progress.start();

                this.form.post('http://5da4468aa6593f001407a4ce.mockapi.io/api/trial/Bills')
                .then(() => {
                    Fire.$emit('afterCreated');
                    $('#addNewCenter').modal('hide');

                    Toast.fire({
                        type: 'success',
                        title: 'Bill created successfully'
                        })
                    this.$Progress.finish();
                })
                .catch(() => {
                    //error
                    this.$Progress.fail();
                    $('#addNewCenter').modal('hide');
                    Toast.fire({
                        type: 'error',
                        title: 'Something is wrong. Try again!'
                      })
                });
            },
            updateBill() {
                this.form.put('http://5da4468aa6593f001407a4ce.mockapi.io/api/trial/Bills/' + this.form.id)
                .then(() => {
                    //success
                    $('#addNewCenter').modal('hide');
                    Swal.fire(
                        'Updated!',
                        'Your bill has been updated.',
                        'success'
                        )
                    Fire.$emit('afterCreated');
                    this.$Progress.finish();
                })
                .catch(() => {
                    //error
                    this.$Progress.fail();
                });
            },
            newModal() {
                this.editmode = false;
                this.form.reset();
                $('#addNewCenter').modal('show');
            },
             editModal(bill) {
                this.editmode = true;
                this.form.reset();
                $('#addNewCenter').modal('show');
                this.form.fill(bill);
            },
            deleteBill(id) {
                Swal.fire({
                    title: 'Are you sure?',
                    text: "You won't be able to revert this!",
                    type: 'warning',
                    showCancelButton: true,
                    confirmButtonColor: '#3085d6',
                    cancelButtonColor: '#d33',
                    confirmButtonText: 'Yes, delete it!'
                    }).then((result) => {

                        // Send request to the server
                        if (result.value) {
                        this.form.delete('http://5da4468aa6593f001407a4ce.mockapi.io/api/trial/Bills/' + id)
                        .then(() => {
                            Swal.fire(
                            'Deleted!',
                            'Your bill has been deleted.',
                            'success'
                            )
                        Fire.$emit('afterCreated');
                        }).catch(() => {
                            Swal("Failed!", "There was something wrong.", "warning");
                        });
                        }
                        
                    })
            },
        }
}
</script>
