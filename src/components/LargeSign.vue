<template>
    <div :id="id" :class="{ 'to-delete': this.delete === true }">
        <div :class="{ 'sign': this.$route.name === 'New Large Sign', 'sign-preview': this.$route.name === 'Large Print Queue' }">
            <div v-if="removable" class="sign-delete" @click="showConfirm"></div>
            <div>
                <table>
                    <tbody>
                        <tr class="bottom-border">
                            <td class="sign-logo">
                                <svg class="dp-logo" x="0px" y="0px" width="342px" height="90px" viewBox="0 0 342 90">
                                    <path fill="#82BC00" d="M89.143,28.296c-3.798-2.095-8.398-3.113-14.063-3.113c-0.784,0-1.543,0.021-2.267,0.052l2.792-21.136  L62.287,8.613c-2.867,0.82-3.515,2.952-3.639,4.207l-2.212,15.068c-2.576,0.841-5.212,1.87-7.863,3.07  c-9.393,4.249-16.77,11.841-19.736,19.752c-1.983,5.29-1.915,10.529,0.197,15.149c3.378,7.39,11.625,11.802,22.062,11.802  c3.963,0,8.026-0.623,12.05-1.846c0.074-0.021-0.989,10.159-0.989,10.159l11.595-3.815c2.866-0.821,3.514-2.953,3.637-4.208  l1.607-12.77c9.488-3.125,17.007-11.534,18.233-20.689C98.159,37.546,95.212,31.642,89.143,28.296z"/>
                                    <path d="M87.191,30.626c-5.947-3.279-13.455-2.812-17.969-2.376l2.389-18.478l-0.293,0.126l-8.684,2.953h0.001  c-0.451,0.175-0.758,0.464-0.953,0.84c-0.004,0.002-0.017,0.037-0.017,0.037c-0.06,0.121-0.106,0.25-0.143,0.386l-0.046,0.02  l-2.27,16.099c-2.918,0.861-5.961,1.994-9.15,3.435C35.875,40.083,27.658,53.34,32.369,63.647  c4.208,9.206,17.152,12.035,29.932,8.156c0.096-0.028,1.271-0.344,1.367-0.375l0.01-0.317l4.709-36.418  c8.101-1.916,14.899-0.909,17.787,2.3c3.839,4.271,0.793,13.261-9.115,16.393l1.852-14.722L76.728,39.4l-4.85,1.605h0.001  c-0.392,0.154-0.664,0.406-0.832,0.735c-0.003,0.002-0.016,0.032-0.016,0.032c-0.101,0.211-0.166,0.446-0.188,0.711l-4.912,37.804  l6.729-2.255c0.991-0.27,1.429-0.769,1.502-1.629l1.824-14.493C91.982,58.037,100.493,37.961,87.191,30.626z M54.405,64.296  c-6.308,0.844-11.712-0.914-13.574-5.396c-2.551-6.138,2.488-13.466,11.108-18.157c2.256-1.227,4.584-2.23,6.138-2.782  L54.405,64.296z"/>
                                    <g>
                                        <path stroke="#000000" stroke-miterlimit="10" d="M111.28,42.058h7.6c6.157,0,11.26,3.3,11.26,9.844c0,6.354-5.131,9.57-11.01,9.57   h-7.85V42.058z M116.272,57.368h2.579c3.716,0,5.769-2.053,5.769-5.466c0-3.41-2.053-5.74-5.769-5.74h-2.579V57.368z"/>
                                        <path stroke="#000000" stroke-miterlimit="10" d="M137.958,53.762v3.578h9.179v4.133h-14.171V42.058h13.59v4.132h-8.597v3.439   h6.711v4.132H137.958z"/>
                                        <path stroke="#000000" stroke-miterlimit="10" d="M157.315,54.955l3.993-12.897h5.047l-6.849,19.415h-4.299l-6.878-19.415h5.048   L157.315,54.955z"/>
                                        <path stroke="#000000" stroke-miterlimit="10" d="M173.398,61.473h-5.047V42.058h5.047V61.473z"/>
                                        <path stroke="#000000" stroke-miterlimit="10" d="M191.426,42.696V47.3c-1.139-0.638-2.413-1.054-4.3-1.054   c-4.076,0-5.657,2.773-5.657,5.547c0,2.801,1.607,5.629,5.657,5.629c1.887,0,3.051-0.165,4.3-0.72v4.41   c-1.498,0.47-3.218,0.638-4.632,0.638c-5.604,0-10.539-2.969-10.539-9.901c0-6.934,4.798-9.929,10.372-9.929   C188.707,41.92,189.845,42.225,191.426,42.696z"/>
                                        <path stroke="#000000" stroke-miterlimit="10" d="M199.799,53.762v3.578h9.181v4.133h-14.172V42.058h13.59v4.132h-8.599v3.439   h6.712v4.132H199.799z"/>
                                        <path fill="#82BC00" d="M217.964,42.058h5.657c3.162,0,7.044,1.276,7.044,6.074c0,4.854-3.882,6.211-6.823,6.211h-2.745v7.13   h-3.133V42.058z M221.097,51.57h2.774c1.636,0,3.521-0.693,3.521-3.438c0-2.718-1.886-3.355-3.521-3.355h-2.774V51.57z"/>
                                        <path fill="#82BC00" d="M236.267,61.473h-3.161V42.058h3.161V61.473z"/>
                                        <path fill="#82BC00" d="M246.75,61.473h-3.079V44.638h-5.354v-2.58h13.783v2.58h-5.351V61.473z"/>
                                        <path fill="#82BC00" d="M259.339,61.75c-2.163,0-4.604-0.389-6.241-0.999v-2.662c1.305,0.444,3.827,1.025,5.991,1.025   c2.329,0,3.966-0.777,3.966-2.856c0-2.412-2.302-2.828-4.687-3.494c-2.44-0.693-4.881-1.747-4.881-5.491   c0-4.437,3.161-5.353,6.322-5.353c2.552,0,4.188,0.332,5.687,0.776l-0.028,2.69c-1.442-0.499-3.273-0.831-4.936-0.831   c-1.777,0-3.911,0.443-3.911,2.607c0,2.107,1.941,2.551,4.104,3.133c2.606,0.722,5.436,1.691,5.436,5.77   C266.161,60.668,262.693,61.75,259.339,61.75z"/>
                                        <path fill="#82BC00" d="M275.591,61.473h-3.079V44.638h-5.354v-2.58h13.784v2.58h-5.351V61.473z"/>
                                        <path fill="#82BC00" d="M290.787,41.92c5.797,0,10.067,3.854,10.067,9.9c0,6.047-4.271,9.93-10.067,9.93   c-5.796,0-10.066-3.883-10.066-9.93C280.721,45.774,284.991,41.92,290.787,41.92z M290.787,58.92c3.827,0,6.796-2.744,6.796-7.1   c0-4.354-2.969-7.072-6.796-7.072c-3.828,0-6.794,2.718-6.794,7.072C283.993,56.176,286.959,58.92,290.787,58.92z"/>
                                        <path fill="#82BC00" d="M303.46,42.058h5.657c3.162,0,7.044,1.276,7.044,6.074c0,4.854-3.882,6.211-6.822,6.211h-2.745v7.13h-3.134   V42.058z M306.594,51.57h2.772c1.637,0,3.522-0.693,3.522-3.438c0-2.718-1.886-3.355-3.522-3.355h-2.772V51.57z"/>
                                    </g>
                                </svg>
                            </td>
                            <td></td>
                        </tr>
                        <tr>
                            <td class="sign-cell right-adjust top-adjust">
                                <input id="sign-product" class="sign-product sign-cell sign-value left" v-model="manufacturer" autocomplete="off">
                            </td>
                            <td></td>
                        </tr>
                        <tr>
                            <td class="sign-cell right-adjust">
                                <input id="sign-model" class="sign-model sign-cell sign-value left" v-model="model" autocomplete="off">
                            </td>
                            <td></td>
                        </tr>
                        <br>
                        <tr>
                            <td class="sign-cell right-adjust">
                                <input id="sign-cpu" class="sign-cpu sign-cell sign-value left" v-model="cpu" autocomplete="off">
                            </td>
                            <td></td>
                        </tr>
                        <tr>
                            <td class="sign-cell right-adjust">
                                <input id="sign-ram" class="sign-ram sign-cell sign-value left" :value="memory + ' RAM'" autocomplete="off">
                            </td>
                            <td></td>
                        </tr>
                        <tr>
                            <td class="sign-cell right-adjust">
                                <input id="sign-drive" class="sign-drive sign-cell sign-value left" :class="{ 'msrp-adjust' : msrp}" :value="capacity" autocomplete="off">
                            </td>
                            <td class="sign-cell left-adjust" v-if="msrp">
                                <!--<span>Original Price:</span>-->
                                <!--<input id="sign-price" class="sign-msrp sign-value" :value="'$' + price" autocomplete="off">-->
                                <input id="sign-msrp" class="sign-msrp sign-value" :value="'$' + msrp" @input="updateSavings()" autocomplete="off">
                                <span class="msrp-text right">Original Price: </span>
                            </td>
                            <td v-else></td>
                        </tr>
                        <tr>
                            <td></td>
                            <td class="sign-cell left-adjust">
                                <input id="sign-price" class="sign-price sign-value" :value="'$' + price" @input="updateSavings()" autocomplete="off">
                            </td>
                        </tr>
                        <tr>
                            <td class="right-adjust">
                                <input id="sign-comment" class="sign-value sign-comment" v-model='comment' autocomplete="off">
                            </td>
                            <td v-if="!msrp"></td>
                            <td class="sign-cell left-adjust" v-else>
                                <!--<span>Original Price:</span>-->
                                <!--<input id="sign-price" class="sign-msrp sign-value" :value="'$' + price" autocomplete="off">-->
                                <span class="savings-text right">You Save: ${{ savings }} ({{ savingsPercent}}%)</span>
                            </td>
                        </tr>
                        <tr>
                            <td></td>
                            <td class="sign-cell left-adjust">
                                <!--<span>Original Price:</span>-->
                                <!--<input id="sign-price" class="sign-msrp sign-value" :value="'$' + price" autocomplete="off">-->
                                <span class="warranty-text right">Free initial set-up, 90-day warranty</span>
                            </td>
                        </tr>
                        <tr v-if="!msrp">
                            <td class="savings-spacer"></td>
                        </tr>
                    </tbody>
                </table>
                <span class="trade-text center">Trade-in your old computer, cell phone, or tablet to save even more!</span>
            </div>
        </div>
    </div>
</template>

<script>
    import database from '@/components/firebaseInit.js';

    export default {
        name: "large-sign",
        props: {
            manufacturer: {
                type: String,
                required: true,
            },
            model: {
                type: String,
                required: true,
            },
            cpu: {
                type: String,
                required: true,
            },
            memory: {
                type: String,
                required: true,
            },
            capacity: {
                type: String,
                required: true,
            },
            drive: {
                type: String,
                required: true,
            },
            msrp: {
                type: Number,
                required: false,
            },
            price: {
                type: Number,
                required: true
            },
            comment: {
                type: String,
                required: false,
            },
            sku: {
                type: String,
                required: true,
            },
            removable: {
                type: Boolean,
                required: false
            }
        },
        data() {
            return {
                savings: this.msrp - this.price,
                savingsPercent: Math.ceil(((this.msrp - this.price) / this.msrp) * 100),
                product: this.manufacturer + " " + this.model,
                productCapacity: this.capacity,
                isPreview: this.$route.name === '',
                delete: false,
                id: 'sign-' + (new Date().getTime()),
                carrierCell: 'carrier-cell' + (new Date().getTime()),
                carrierId: 'carrier-' + (new Date().getTime()),
                commentCell: 'comment-cell' + (new Date().getTime()),
                commentId: 'comment-' + (new Date().getTime()),
            }
        },
        methods: {
            updateSavings() {
                let msrp = document.querySelector("#sign-msrp").value.replace("$", "");
                let price = document.querySelector("#sign-price").value.replace("$", "");

                if (msrp === "") {
                    msrp = "0";
                }

                if (price === "") {
                    price = "0";
                }

                this.savings = (msrp - price).toFixed(2);
                this.savingsPercent = Math.ceil(((msrp - price) / msrp) * 100);

                this.msrp = msrp;
                this.price = price;
            },
            showConfirm() {
                this.delete = true;

                this.$root.$emit('openConfirm', {
                    closed: false,
                    text: 'Are you sure you want to delete this sign?',
                    type: 'warning',
                    dimScreen: true
                });
            },
            showModal(message, type) {
                this.$root.$emit('openModal', {
                    closed: false,
                    text: message,
                    type: type
                })
            }
        },
        created() {

        },
        mounted() {

        },
    }
</script>

<style scoped>

    .msrp-adjust {
        position: relative;
        top: -7px;
    }

    .top-adjust {
        padding-top: 10px;
    }

    .right-adjust {
        padding-left: 20px;
    }

    .left-adjust {
        padding-right: 10px;
    }

    .msrp-text {
        font-size: 18px;
        line-height: 47px;
        position: relative;
        top: 4px;
        color: #aaa;
    }

    .savings-text {
        font-size: 20px;
        color: #aaa;
        position: relative;
        bottom: 5px;
    }

    .savings-spacer {
        padding: 21.5px;
    }

    .warranty-text {
        font-size: 16px;
        color: #f89728;

    }

    .sign-comment {
        font-size: 16px;
        text-transform: uppercase;
    }

    .trade-text {
        position: relative;
        top: 5px;
        left: 110px;
    }

    table {
        border: none;
    }

    .sign {
        height: 30em;
        width: 42.15em;
        position: absolute;
        left: 30.15em;
        background-color: #fff;
        zoom: 50%;
    }

    .sign-preview {
        height: 31.97em;
        width: 44.77em;
        background-color: #fff;
        max-height: 32em;
        zoom: 96%;
    }

    .dp-logo {
        width: inherit;
        height: inherit;
        display: block;
    }

    .sign-logo {
        height: 4.25em;
        width: 15.75em;
        background-size: 12em;
        background-position: 1em;
        display: table;
    }

    .bottom-border {
        border-bottom: 6px solid #8dc63f !important;
    }

    .sign-cell {
        border-bottom: none !important;
        font-family: 'Metropolis SemiBold', sans-serif;
    }

    input.sign-model {
        width: 150%!important;
    }

    input.sign-value:not(.sign-msrp) {
        padding: 0;
        width: 100%;
    }

    input:disabled {
        background: white;
    }

    input:disabled:hover {
        color: #999;
        background: #f0f0f0;
        cursor: not-allowed;
    }

    .sign-value {
        line-height: 1.42857143;
        color: #231f20;
        display: block;
        border: none;
        transition: border-color ease-in-out .15s,box-shadow ease-in-out .15s;
    }

    .sign-price {
        font-size: 64px;
        font-family: "Metropolis Black" !important;
        border-bottom: none !important;
        text-align: right;
        width: 4.5em!important;
        float: right;
    }

    .sign-product {
        font-size: 28px;
        font-family: "Metropolis Bold" !important;
    }

    .sign-model {
        font-size: 22px;
    }

    .sign-cpu, .sign-ram, .sign-drive {
        font-size: 22px;
    }

    .sign-msrp {
        font-size: 32px;
        font-family: 'Metropolis Bold';
        text-decoration: line-through;
        text-decoration-color: red;
        text-align: right;
        float: right;
        width: 4.8em!important;
    }

    .sign-sku {
        text-align: right !important;
        font-size: 12px;
        position: relative;
        border-bottom: none !important;
        text-transform: uppercase;
    }

    .unlocked {
        background-color: yellow;
    }

    .sign-delete:hover {
        background-color: rgba(255,0,0,0.55);
        cursor: pointer;
    }

    .sign-delete {
        width: 20px;
        height: 12px;
        position: absolute;
        left: 43.4em;
    }

    .sign-delete:hover::before {
        content: '\00d7';
        position: absolute;
        left: 0.27em;
        bottom: -0.45em;
        color: rgba(255,255,255,0.75);
        font-size: 1.2666666667em;
    }

</style>