<template>
    <div id="storestockedit" v-title="title">
        <modal></modal>
        <div class="column form-column">
            <div class="column-header header-btm-border">
                <div class="action-row">
                    <div class="right">
                        <return-btn @click.native="back()"></return-btn>
                    </div>
                    <div class="font-xl">
                        Edit Store Stock
                    </div>
                </div>
            </div>
            <div id="column-content-form">
                <div id="column-container">
                    <div class="row product-form">
                        <div class="row-container">
                            <form id="sign-form" action="" autocomplete="off">
                                <form-input elementId="technician" label="Technician" labelWidth="15" inputWidth="25"
                                            type="text" small double required placeholder="First Name" placeholderTwo="Last Name"></form-input>
                                <autocomplete elementId="manufacturer" :items="manufacturers" label="Manufacturer"
                                              labelWidth="15" inputWidth="50" type="text" required
                                              v-model="manufacturer"></autocomplete>
                                <autocomplete elementId="model" :items="models" label="Product Model" labelWidth="15"
                                              inputWidth="50" type="text" required v-model="model"></autocomplete>
                                <autocomplete elementId="carrier" :items="carriers" label="Carrier" labelWidth="15"
                                              inputWidth="50" type="text" v-model="carrier"></autocomplete>
                                <autocomplete elementId="capacity" :items="capacities" label="Storage Capacity"
                                              labelWidth="15" inputWidth="50" type="text"
                                              v-model="capacity"></autocomplete>
                                <form-input elementId="actions" label="Actions Performed" labelWidth="15"
                                            inputWidth="25" type="text" small double select required
                                            :options="options"></form-input>
                                <form-input elementId="comments" label="Comments" labelWidth="15" inputWidth="50"
                                            type="text" textarea></form-input>
                                <form-input elementId="commentHistory" label="Comment History" labelWidth="15" inputWidth="50"
                                            type="text" disabled textarea></form-input>
                                <form-input elementId="sku" label="Product SKU" labelWidth="15" inputWidth="50"
                                            type="text" required
                                            placeholder="Hint: You can scan the barcode!"></form-input>
                                <date-picker></date-picker>
                                <br>
                                <h4 style="font-family: 'Metropolis Light'">Additional Parameters</h4>
                                <div class="form-group">
                                    <label class="input-15">Sold</label>
                                    <div class="input-50" style="position: relative; left: 125px;">
                                        <input id="sold" type="checkbox" class="form-control-sm" v-model="sold">
                                    </div>
                                </div>
                                <div class="form-group">
                                    <label class="input-15">Ready for Floor</label>
                                    <div class="input-50" style="position: relative; left: 52px;">
                                        <input id="ready" type="checkbox" class="form-control-sm" v-model="ready">
                                    </div>
                                </div>
                            </form>
                        </div>
                    </div>
                    <hr class="border-light">
                    <div class="row">
                        <div class="btn-toolbar-left">
                            <btn-33 primary content="Save" @click.native="save()"></btn-33>
                            <btn-33 content="Cancel" @click.native="back()"></btn-33>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import Btn33 from '@/components/Btn33.vue'
    import ReturnBtn from '@/components/ReturnBtn.vue'
    import FormInput from '@/components/FormInput.vue'
    import Autocomplete from '@/components/Autocomplete.vue'
    import Sign from '@/components/Sign.vue'
    import Modal from '@/components/Modal.vue'
    import ConfirmDialog from '@/components/ConfirmDialog.vue'
    import DatePicker from '@/components/DatePicker.vue'
    import database from '@/components/firebaseInit.js'

    export default {
        name: "EditStoreStock",
        components: {
            Btn33,
            ReturnBtn,
            FormInput,
            Autocomplete,
            Sign,
            Modal,
            ConfirmDialog,
            DatePicker
        },
        data() {
            return {
                manufacturers: [],
                models: [],
                carriers: [],
                capacities: [],
                title: 'Store Stock - Editing ' + this.$route.params.id + ' | Device Pitstop - Maple Grove',
                manufacturer: '',
                model: '',
                carrier: '',
                capacity: '',
                comments: '',
                sku: '',
                options: ['Cellbie', 'Cleaned', 'DPOS', 'Replaced Battery', 'Replaced Screen', 'Windows installation'],
                sold: false,
                ready: false
            }
        },
        methods: {
            updateVModels() {
                let manufacturer = document.getElementById("manufacturer").value;
                let model = document.getElementById("model").value;
                let carrier = document.getElementById("carrier").value;
                let capacity = document.getElementById("capacity").value;
                let sku = document.getElementById("sku").value.toUpperCase();

                this.manufacturer = manufacturer;
                this.model = model;
                this.carrier = carrier;
                this.capacity = capacity;
                this.sku = sku;
            },
            back() {
                this.$router.go(-1);
            },
            save() {
                let self = this;
                let form = document.getElementById('sign-form');

                if (form.reportValidity()) {

                    // Sourcing these values manually instead of using v-model because v-model doesn't get the whole value when tabbing to autofill
                    let firstName = document.getElementById("technician").value;
                    let lastName = document.getElementById("technicianTwo").value;
                    let manufacturer = document.getElementById("manufacturer").value;
                    let model = document.getElementById("model").value;
                    let carrier = document.getElementById("carrier").value;
                    let capacity = document.getElementById("capacity").value;
                    let actions = document.getElementById("actionsTwo").value;
                    let comments = document.getElementById("comments").value;
                    let sku = document.getElementById("sku").value;

                    let product = {
                        firstName: firstName,
                        lastName: lastName,
                        manufacturer: manufacturer,
                        model: model,
                        carrier: carrier,
                        capacity: capacity,
                        actions: actions,
                        comments: comments,
                        sku: sku,
                        sold: this.sold,
                        ready: this.ready,
                        day: Number(this.getDay()),
                        month: Number(this.getMonth()),
                        year: Number(this.getYear()),
                        date: new Date(this.getDate()),
                        time: new Date().toLocaleTimeString()
                    };

                    database.collection('devices').doc(this.$route.params.id).get()
                        .then(docSnapshot => {
                            // This will only be true if a SKU already exists and someone is trying to enter it again.
                            // In which case, it will be processed as an update.
                            if (docSnapshot.exists) {
                                // Compare JSON objects to see if there has been any updates
                                let doc = docSnapshot.data();
                                let updates = self.compareJSON(doc, product);

                                // console.log("---");
                                // console.log(updates.comments);
                                // console.log(updates.comments[0]);

                                // If updates occurred...
                                if (updates.update) {

                                    let update = {};

                                    // If updates[key] is true (meaning that field has been updated), set update[key] to product[key]
                                    for (let key in updates) {
                                        if (updates[key] && key !== 'update') {
                                            update[key] = product[key];
                                        }
                                    }

                                    update.comments = updates.comments;

                                    // If SKU was the only thing updated...
                                    if (update.sku) {
                                        let oldSku = self.$route.params.id;
                                        database.collection('devices').doc(oldSku).delete()
                                            .then(() => {
                                                let newDoc = doc;

                                                for (let key in update) {
                                                    newDoc[key] = update[key];
                                                }

                                                database.collection('devices').doc(newDoc.sku).set(newDoc)
                                                    .then(() => {
                                                        console.log("Document written with ID: ", newDoc.sku);
                                                        self.routeTo('/storestock')
                                                    })
                                                    .catch((error) => {
                                                        console.error("Error adding document: ", error);

                                                        self.$root.$emit('openModal', {
                                                            closed: false,
                                                            text: 'An error occurred while saving to the database.',
                                                            type: 'error'
                                                        });
                                                    });

                                            })
                                    }

                                    // Otherwise, save it as normal
                                    else {
                                        database.collection('devices').doc(sku).update(update)
                                            .then(() => {
                                                console.log("Document written with ID: ", sku);
                                                self.routeTo('/storestock')
                                            })
                                            .catch((error) => {
                                                console.error("Error adding document: ", error);

                                                self.$root.$emit('openModal', {
                                                    closed: false,
                                                    text: 'An error occurred while saving to the database.',
                                                    type: 'error'
                                                });
                                            });
                                    }
                                }
                            }
                        })
                }
            },
            compareJSON(a, b) {
                let updates = {
                    update: false
                };

                // Save the new comment and then clear new JSON's comment so it can be added to.
                let newComment = b.comments;
                b.comments = '';

                // Iterate through the keys in a and check if b has an equivalent key which is different
                for (let key in a) {
                    // Where a is the original JSON and b is the new JSON...
                    if (a[key] !== b[key]) {
                        if (key !== 'comments') {
                            // The update property shows that at least one thing in the JSON has been updated.
                            updates.update = true;

                            // This property has been updated in the JSON.
                            updates[key] = true;

                            // Log the change in the comments section (ex. model: iPhone 6 --> iPhone 6S)
                            b.comments += this.formatComment(key, a, b);
                        }
                    }
                    else {
                        // This property has not been updated in the JSON.
                        updates[key] = false;
                    }
                }

                // If update occurred...
                if (updates.update) {
                    let date = this.getDate();
                    // If technician is not defined, technician becomes Anonymous User. This should never happen, but just in case it does...
                    let technician = ((b.firstName + " " + b.lastName) !== " " ? b.firstName + " " + b.lastName : "Anonymous User");

                    b.comments = "== Update on (" + date + ") by " + technician + " ==\n" + b.comments + newComment + '\n';

                    // Put update at the top of the comment history.
                    updates.comments = [];
                    updates.comments.unshift(b.comments);

                    // Do not concat old comment if it does not exist. Otherwise it just puts "" in an array index.
                    if (a.comments !== "") {
                        updates.comments = updates.comments.concat(a.comments);
                    }
                }

                return updates;
            },
            formatComment(key, originalJSON, newJSON) {
                if (key !== 'day' && key !== 'month' && key !== 'year' && key !== 'date' && key !== 'firstName' && key !== 'lastName' && key !== 'time') {
                    let originalValue = originalJSON[key];
                    let newValue = newJSON[key];
                    let change;

                    if (newValue === "") {
                        change = 'DELETED';
                    }

                    else {
                        change = newValue;
                    }

                    return key + ": " + originalValue + ' --> ' + change + '\n';
                }

                else {
                    return "";
                }
            },
            getDay() {
                return document.getElementById("date").value.split("/")[1];
            },
            getMonth() {
                return document.getElementById("date").value.split("/")[0];
            },
            getYear() {
                return document.getElementById("date").value.split("/")[2];
            },
            getDate() {
                return document.getElementById("date").value;
            },
            getFullDate() {
                let date = new Date();
                let month = date.getMonth() + 1;
                let day = date.getDate();
                let year = date.getFullYear();

                return month + "/" + day + "/" + year;
            },
            routeTo(route) {
                this.$router.push(route);
            },
            updateOptions() {
                // Using this function prevents the ability to populate #actionsTwo with text with JavaScript. It seems to be updating the v-model of the component, and even updating it after the fact doesn't fix it.

                let manufacturer = document.querySelector("#manufacturer").value;
                let product = document.querySelector("#model").value;
                let phoneOptions = ['iPhone', 'iPad', 'Galaxy'];
                let computerOptions = ['ASUS', 'Acer', 'Alienware', 'Compaq', 'Dell', 'Gateway', 'HP', 'iMac', 'Lenovo', 'LG', 'MacBook', 'Microsoft', 'MSI', 'Samsung', 'Toshiba'];

                // If iPhone, iPad or Galaxy
                if (phoneOptions.some(substring => product.includes(substring))) {
                    this.options = ['Battery replacement', 'Cellbie', 'Cleaned', 'Dock port replacement', 'Proxy replacement', 'Screen replacement'];
                }

                // If made by certain manufacturers or is an Apple computer
                else if (computerOptions.some(substring => manufacturer.includes(substring) && !product.includes(substring)) || (computerOptions.some(substring => product.includes(substring)))) {
                    this.options = ['Cleaned', 'DPOS', 'Extended hard drive', 'Hard drive installation', 'OS activation', 'OS installation'];
                }
            }
        },
        created() {
            database.collection('manufacturer').orderBy('manufacturer').get()
                .then(snapshot => {
                    snapshot.forEach(doc => {
                        let manufacturer = doc.data().manufacturer;
                        this.manufacturers.push(manufacturer);
                    })
                });

            database.collection('product').orderBy('product').get()
                .then(snapshot => {
                    snapshot.forEach(doc => {
                        let product = doc.data().product;
                        this.models.push(product);
                    })
                });

            database.collection('carrier').orderBy('carrier').get()
                .then(snapshot => {
                    snapshot.forEach(doc => {
                        let carrier = doc.data().carrier;
                        this.carriers.push(carrier);
                    })
                });

            database.collection('capacity').orderBy('capacity', "asc").get()
                .then(snapshot => {
                    snapshot.forEach(doc => {
                        let capacity = String(doc.data().capacity + "GB");
                        this.capacities.push(capacity);
                    })
                });

            database.collection('devices').doc(this.$route.params.id).get()
                .then((docSnapshot) => {
                    let doc = docSnapshot.data();

                    // This is really not the ideal way to do this, but trying to modify the v-model just gives you undefined.
                    document.getElementById("technician").value = doc.firstName;
                    document.getElementById("technicianTwo").value = doc.lastName;
                    document.getElementById("manufacturer").value = doc.manufacturer;
                    document.getElementById("model").value = doc.model;
                    document.getElementById("carrier").value = doc.carrier;
                    document.getElementById("capacity").value = doc.capacity;
                    document.getElementById("actionsTwo").value = doc.actions;

                    if (doc.comments && doc.comments.includes("\n")) {

                        document.getElementById("commentHistory").value = doc.comments.join('\n');
                    }
                    else if (doc.comments) {
                        document.getElementById("commentHistory").value = doc.comments.join("");
                    }
                    document.getElementById("sku").value = doc.sku;
                    document.getElementById("date").value = this.getFullDate();
                    this.sold = doc.sold;
                    this.ready = doc.ready;

                    this.updateVModels();
                });

        },
        beforeUpdate() {
            // This was not working for some reason.
            // I believe it's because populating the fields updates the v-models and it messes with the function's ability to check for something.
            // I'm not sure. I just know it doesn't work.
            // this.updateOptions();
        }
    }
</script>

<style scoped>
    #sign-form {
        margin-top: -1px;
    }

    .action-row {
        padding: 16px 10px 15px 18px;
    }

    .form-column {
        width: 65.5%;
    }

    .column {
        top: 3.1rem;
    }

    .btn-toolbar-left {
        margin-left: 0.75rem;
    }

    .btn {
        margin-left: 0.75rem;
    }

    #column-content-form {
        max-width: 1040px;
        margin-top: 25px;
        margin-right: 32px;
    }

    .product-form {
        padding: 0 20px 50px;
    }

    .border-light {
        border-top: none;
        border-bottom: 1px solid #F3F3F3;
        margin: 0 0 25px 15px;
    }

    hr {
        border: 0;
        background: #e5e5e5;
        margin: 0 0 20px;
    }

    .form-group {
        margin-bottom: 15px;
    }

    .form-group label {
        display: inline-block;
        padding-right: 15px;
    }

    .form-group div {
        display: inline-block;
    }
</style>