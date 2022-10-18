<template>
    <div class="row mt-5">
        <div class="col-lg-4 col-md-6 col-sm-7 col-xs-4 mb-3">
            <div class="p-4 card shadow rounded">
                <form onsubmit="return false;">
                <!-- Network -->
                    <div class="row">
                        <div class="col-12">
                            <h5 class="text-dark mb-3">Network</h5>
                            <div class="input-group mb-3">
                                <input type="text" class="form-control" pattern="[0-9]{1-3}" min="0" max="255" maxlength="3" placeholder="172" aria-label="Octeto 1" v-model="net.oct1" required>
                                <span class="input-group-text">.</span>
                                <input type="text" class="form-control" pattern="[0-9]{1-3}" min="0" max="255" maxlength="3" placeholder="16" aria-label="Octeto 2" v-model="net.oct2" required>
                                <span class="input-group-text">.</span>
                                <input type="text" class="form-control" pattern="[0-9]{1-3}" min="0" max="255" maxlength="3" placeholder="1" aria-label="Octeto 3" v-model="net.oct3" required>
                                <span class="input-group-text">.</span>
                                <input type="text" class="form-control" pattern="[0-9]{1-3}" min="0" max="255" maxlength="3" placeholder="0" aria-label="Octeto 4" v-model="net.oct4" required>
                            </div>
                        </div>
                    </div>
                    <hr>
                    <div class="row mb-2 align-items-center">
                        <div class="col-5">
                            <h5 class="text-dark">Subnet list</h5>
                        </div>
                        <div class="col-4">
                            
                        </div>
                        <div class="col-2">
                            <button class="btn btn-success" @click="addRow">
                                Add
                            </button>
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-6">
                            <label class="form-label">Subnet Name</label>
                        </div>
                        <div class="col-4">
                            <label class="form-label"># Hosts</label>
                        </div>
                        <div class="col-1">
                        </div>
                    </div>
                    <div class="row" id="subnetlist">
                        <div class="row px-3 align-items-center">
                            <div class="col-6">
                                <span class="form-control text-info">Subnet 1</span>
                            </div>
                            <div class="col-4">
                                <input type="number" min="1" class="form-control" name="host[]" required>
                            </div>
                            <div class="col-1 pl-1">
                                <button class="btn btn-danger btn-sm" disabled>
                                    <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-trash-fill" viewBox="0 0 16 16">
                                        <path d="M2.5 1a1 1 0 0 0-1 1v1a1 1 0 0 0 1 1H3v9a2 2 0 0 0 2 2h6a2 2 0 0 0 2-2V4h.5a1 1 0 0 0 1-1V2a1 1 0 0 0-1-1H10a1 1 0 0 0-1-1H7a1 1 0 0 0-1 1H2.5zm3 4a.5.5 0 0 1 .5.5v7a.5.5 0 0 1-1 0v-7a.5.5 0 0 1 .5-.5zM8 5a.5.5 0 0 1 .5.5v7a.5.5 0 0 1-1 0v-7A.5.5 0 0 1 8 5zm3 .5v7a.5.5 0 0 1-1 0v-7a.5.5 0 0 1 1 0z" />
                                    </svg>
                                </button>
                            </div>
                        </div>
                    </div>
                    <hr>
                    <div class="row">
                        <div class="col">
                            <button class="btn btn-primary w-100" type="submit" @click="calculate">
                                Calculate
                            </button>
                        </div>
                    </div>
                </form>
            </div>
        </div>
        <div class="col" v-if="this.res.length === 0">
            <div class="card rounded shadow">
                <div class="card-header">
                    <h5 class="text-dark">Notes</h5>
                </div>
                <div class="card-body">
                    <li>Error al eliminar una subnet de la lista (botón no funciona).</li>
                    <li>Error al calcular subnets WAN.</li>
                    <li>Error al introducir algunos valores.</li>
                    <li>Errores de validación.</li>
                </div>
                <div class="card-footer">
                    <span>Se está trabajando en los errores comentados anteriormente, diculpe las molestias.</span>
                </div>
            </div>
        </div>
        <div class="col" v-else>
            <Results :resp="res"></Results>
        </div>
    </div>
</template>

<script>
import Vue from 'vue';
import Results from '../components/Results.vue';

export default Vue.extend({
    name: "Form",
    components: {
    Results,
},
    data() {
        return {
            net: {
                oct1: 192,
                oct2: 168,
                oct3: 1,
                oct4: 0
            },
            rows: {
                numCampMax: 13,
                x: 2
            },
            newNet: {
                oct1: 0,
                oct2: 0,
                oct3: 0,
                oct4: 0,
                mask: 0
            },
            res: [],
            auxResults: [],
            auxNet: {
                oct1: 0,
                oct2: 0,
                oct3: 0,
                oct4: 0
            },
        }
    },
    methods: {
        resetData(){
            this.net = {
                oct1: 192,
                oct2: 168,
                oct3: 1,
                oct4: 0
            };
            this.newNet = {
                oct1: 0,
                oct2: 0,
                oct3: 0,
                oct4: 0,
                mask: 0
            };
            this.auxResults = [];
            this.auxNet = {
                oct1: 0,
                oct2: 0,
                oct3: 0,
                oct4: 0
            };
        },
        addRow(e) {
            e.preventDefault();
            if (this.rows.x < this.rows.numCampMax) {
                let con = document.getElementById("subnetlist");
                let div = document.createElement("div");
                div.classList.add("row", "px-3", "mt-3", "align-items-center");
                div.innerHTML = "<div class=\"col-6\">\
                                    <span class=\"form-control text-info\"> Subnet " + this.rows.x + "</span>\
                                </div>\
                                <div class=\"col-4\">\
                                    <input type=\"number\" min=\"1\" class=\"form-control\" name=\"host[]\" required>\
                                </div>\
                                <div class=\"col-1 pl-1\">\
                                    <button class=\"btn btn-danger btn-sm\" v-on:click=\"deleteRow\">\
                                        <svg xmlns=\"http://www.w3.org/2000/svg\" width=\"16\" height=\"16\" fill=\"currentColor\" class=\"bi bi-trash-fill\" viewBox=\"0 0 16 16\">\
                                            <path d=\"M2.5 1a1 1 0 0 0-1 1v1a1 1 0 0 0 1 1H3v9a2 2 0 0 0 2 2h6a2 2 0 0 0 2-2V4h.5a1 1 0 0 0 1-1V2a1 1 0 0 0-1-1H10a1 1 0 0 0-1-1H7a1 1 0 0 0-1 1H2.5zm3 4a.5.5 0 0 1 .5.5v7a.5.5 0 0 1-1 0v-7a.5.5 0 0 1 .5-.5zM8 5a.5.5 0 0 1 .5.5v7a.5.5 0 0 1-1 0v-7A.5.5 0 0 1 8 5zm3 .5v7a.5.5 0 0 1-1 0v-7a.5.5 0 0 1 1 0z\" />\
                                        </svg>\
                                    </button>\
                                </div>";
                con.appendChild(div);
                this.rows.x++;
            }
        },
        delete: function (event) {
            if (event) {
                alert("hey");
            }
        },
        calculate(e) {
            e.preventDefault();
            const hostList = document.querySelectorAll("input[name=\"host[]\"]");
            let hosts = 0;
            let cont = 1;
            hostList.forEach((host) => {
                let band = false;
                let i = 2;
                // Calcula los hosts que se ajustan
                do {
                    if (Math.pow(2, i) >= host.value) {
                        hosts = Math.pow(2, i);
                        band = true;
                    } else {
                        i++;
                    }
                } while (band == false);
                // Calcula submascara
                this.newNet.mask = 32 - i;
                // Calcula red
                if (hosts > 255) {
                    this.newNet.oct1 = this.net.oct1;
                    this.newNet.oct2 = this.net.oct2;
                    this.newNet.oct3 = this.net.oct3 + Math.floor(hosts / 255);
                    this.newNet.oct4 = this.net.oct4 + hosts % 255;
                } else {
                    this.newNet.oct1 = this.net.oct1;
                    this.newNet.oct2 = this.net.oct2;
                    this.newNet.oct3 = this.net.oct3 + Math.floor(hosts / 255);
                    this.newNet.oct4 = this.net.oct4 + hosts;
                }
                // Llama al método para generar las subnets
                this.getSubnets(cont);
                this.changueSubnets();
                cont++;
            });

            this.sendResults();
            this.resetData();
        },
        getSubnets(item) {
            let netw = (this.net.oct1) + "." + (this.net.oct2) + "." + (this.net.oct3) + "." + (this.net.oct4) + "/" + this.newNet.mask;
            let firs = (this.net.oct1) + "." + (this.net.oct2) + "." + (this.net.oct3) + "." + (this.net.oct4 + 1);
            let last = (this.newNet.oct1) + "." + (this.newNet.oct2) + "." + (this.newNet.oct3) + "." + (this.newNet.oct4 - 3);
            let gatw = (this.newNet.oct1) + "." + (this.newNet.oct2) + "." + (this.newNet.oct3) + "." + (this.newNet.oct4 - 2);
            let brct = (this.newNet.oct1) + "." + (this.newNet.oct2) + "." + (this.newNet.oct3) + "." + (this.newNet.oct4 - 1);
            let col = '';
                col = this.getColor();

            this.auxResults.push({
                id: item,
                subnet: netw,
                first: firs,
                last: last,
                gateway: gatw,
                broadcast: brct,
                color: col
            });

            // console.log(netw + "\n" + firs + "\n" + last + "\n" + gatw + "\n" + brct + "\n---------------------------\n");
        },
        changueSubnets() {
            this.net.oct1 = this.newNet.oct1;
            this.net.oct2 = this.newNet.oct2;
            this.net.oct3 = this.newNet.oct3;
            this.net.oct4 = this.newNet.oct4;
        },
        sendResults() {
            this.res = [];
            this.res = this.auxResults;
        },
        getColor() {
            var ArrayColors = ['#FFB6AE','#FFE1AE','#FAFCAF','#B0F2C2','#B0C2F2','#E9B0F2','#BDF1E5','#C4E7F1','#D1DBE2']
            var rand = Math.floor(Math.random()*ArrayColors.length);
            var col = ArrayColors[rand];
 
            return col;    
        }
    },
})
</script>
