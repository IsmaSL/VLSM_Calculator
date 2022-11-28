<template>
    <div>
        <HeaderVue></HeaderVue>
        <main class="container">
            <div class="row mt-5 justify-content-center">
                <!-- Primer Formulario -->
                <div class="col-xl-4 col-lg-5 col-md-7 col-sm-8 col-xs-10 mb-3 form-show" id="form1"
                    v-bind:class="{ 'form-hidde': forms.form1 === true }">
                    <div class="p-4 card shadow rounded">
                        <div class="row justify-content-center">
                            <h4>Calculadora VLSM</h4>
                        </div>
                        <div class="row">
                            <!-- Formulario -->
                            <form onsubmit="return false;">
                                <!-- Red -->
                                <div class="col">
                                    <label class="mt-3">Dirección IP Principal:</label>
                                    <div class="input-group mb-3">
                                        <input type="text" class="form-control" pattern="[0-9]{1,3}" min="0" max="255"
                                            maxlength="3" placeholder="172" aria-label="Octeto 1" v-model="net.oct1"
                                            required>
                                        <span class="input-group-text">.</span>
                                        <input type="text" class="form-control" pattern="[0-9]{1,3}" min="0" max="255"
                                            maxlength="3" placeholder="16" aria-label="Octeto 2" v-model="net.oct2"
                                            required>
                                        <span class="input-group-text">.</span>
                                        <input type="text" class="form-control" pattern="[0-9]{1,3}" min="0" max="255"
                                            maxlength="3" placeholder="1" aria-label="Octeto 3" v-model="net.oct3"
                                            required>
                                        <span class="input-group-text">.</span>
                                        <input type="text" class="form-control" pattern="[0-9]{1,3}" min="0" max="255"
                                            maxlength="3" placeholder="0" aria-label="Octeto 4" v-model="net.oct4"
                                            required>
                                    </div>
                                </div>
                                <!-- Cantidad de host -->
                                <div class="col">
                                    <label for="">Número de Redes LAN:</label>
                                    <div class="">
                                        <input type="text" class="form-control" pattern="[0-9]{1,3}" min="1" max="12"
                                            maxlength="2" placeholder="12" aria-label="Subredes" v-model="subn.nSub"
                                            required>
                                    </div>
                                </div>
                                <!-- Calcular WANs -->
                                <div class="col">
                                    <div class="form-check mt-2">
                                        <input class="form-check-input" type="checkbox" id="flexCheckDefault" @change="showInputWans()">
                                        <label class="form-check-label" for="flexCheckDefault">
                                            ¿Calcular WANs?
                                        </label>
                                    </div>
                                    <div class="mt-1" id="wans" v-bind:class="{ 'form-hidde': forms.inWan === false }">
                                        <input type="text" class="form-control" pattern="[0-9]{1,3}" min="1" max="30"
                                            maxlength="2" placeholder="Número de Redes WAN" aria-label="WANs" v-model="wans.nWan"
                                            required>
                                    </div>
                                </div>
                                <hr>
                                <div class="col">
                                    <div class="row">
                                        <div class="col"><button class="btn btn-warning w-100" type="submit">
                                                Borrar
                                            </button>
                                        </div>
                                        <div class="col"><button class="btn btn-primary w-100" type="submit"
                                                @click="validate(1)">
                                                Siguiente
                                            </button>
                                        </div>
                                    </div>
                                </div>
                            </form>
                        </div>
                    </div>
                </div>
                <!-- Segundo Formulario -->
                <div class="col-xl-4 col-lg-5 col-md-7 col-sm-8 col-xs-10 mb-3 form-show" id="form2"
                    v-bind:class="{ 'form-hidde': forms.form2 === true }">
                    <div class="p-4 card shadow rounded">
                        <div class="row justify-content-center">
                            <h4>Calculadora VLSM</h4>
                        </div>
                        <div class="row">
                            <!-- Formulario -->
                            <form onsubmit="return false;">
                                <div class="col">
                                    <p class="mt-3">Rellen los campos con el número de host a utilizar en cada una de las subredes</p>
                                    <div class="row">
                                        <div class="col-7">
                                            <label class="form-label">Subnet Name</label>
                                        </div>
                                        <div class="col-5">
                                            <label class="form-label"># Hosts</label>
                                        </div>
                                    </div>
                                    <div class="row" id="subnetlist">
                                    </div>
                                    <hr>
                                    <div class="row">
                                        <div class="col">
                                            <button class="btn btn-primary w-100" type="submit"
                                                @click="ocultarForm(2)">
                                                Regresar
                                            </button>
                                        </div>
                                        <div class="col">
                                            <button class="btn btn-success w-100" type="submit"
                                                @click="validate(2)">
                                                Calcular
                                            </button>
                                        </div>
                                    </div>
                                </div>
                            </form>
                        </div>
                    </div>
                </div>
                <!-- Resultados -->
                <div class="col-xl-8 mb-3 form-show" id="results"
                    v-bind:class="{ 'form-hidde': forms.table === true }">
                    <div class="p-4 card shadow rounded">
                        <div class="row justify-content-center">
                            <h4>Calculadora VLSM</h4>
                        </div>
                        <hr>
                        <div class="row justify-content-center">
                            <button class="btn btn-primary">Calcular Nueva Red</button>
                        </div>
                        <hr>
                        <!-- LANs -->
                        <div class="row justify-content-center">
                            <div class="text-center">
                                <h5 class="text-success">Redes LAN</h5>
                            </div>
                            <div class="table-responsive">
                                <div class="col ">
                                    <table class="table table-light table-striped table-hover">
                                        <thead class="text-center">
                                            <tr>
                                                <th>#</th>
                                                <th>Subred</th>
                                                <th>Primer Host</th>
                                                <th>Último Host</th>
                                                <th>Gateway</th>
                                                <th>Broadcast</th>
                                            </tr>
                                        </thead>
                                        <tbody class="table-group-divider text-center">
                                            <tr v-for="(s, key) in results" :key="key">
                                                <td>{{ s.i }}</td>
                                                <td>{{ s.r }}</td>
                                                <td>{{ s.p }}</td>
                                                <td>{{ s.u }}</td>
                                                <td>{{ s.g }}</td>
                                                <td>{{ s.b }}</td>
                                            </tr>
                                        </tbody>
                                    </table>
                                </div>
                            </div>
                        </div>
                        <hr  v-if="forms.inWan">
                        <!-- WANs -->
                        <div class="row justify-content-center" v-if="forms.inWan">
                            <div class="text-center">
                                <h5 class="text-primary">Redes WAN</h5>
                            </div>
                            <div class="table-responsive">
                                <div class="col ">
                                    <table class="table table-light table-striped table-hover">
                                        <thead class="text-center">
                                            <tr>
                                                <th>#</th>
                                                <th>Subred</th>
                                                <th>Primer Host</th>
                                                <th>Último Host</th>
                                                <th>Broadcast</th>
                                            </tr>
                                        </thead>
                                        <tbody class="table-group-divider text-center">
                                            <tr v-for="(w, key2) in resultsWans" :key="key2">
                                                <td>{{ w.i }}</td>
                                                <td>{{ w.r }}</td>
                                                <td>{{ w.p }}</td>
                                                <td>{{ w.u }}</td>
                                                <td>{{ w.b }}</td>
                                            </tr>
                                        </tbody>
                                    </table>
                                </div>
                            </div>
                        </div>  
                    </div>
                </div>
            </div>
            <!-- <Form></Form> -->
        </main>
        <footer class="bg-light text-center text-lg-start">
            <div class="text-center bg-dark p-3">
                © 2022 Copyright:
                <a class="text-dark" href="https://mdbootstrap.com/">
                    <span class="text-white">Ismael SL</span>
                </a>
            </div>
        </footer>
        <!-- <footer class="p-3 text-center">
        ISL @ 2022
    </footer> -->
    </div>
</template>

<script lang="js">
import Vue from 'vue';
import HeaderVue from '~/components/Header.vue';
import Form from '~/components/Form.vue';

export default Vue.extend({
    name: 'IndexPage',
    components: {
        HeaderVue,
        Form,
    },
    data() {
        return {
            forms: {
                form1: false,
                form2: true,
                inWan: false,
                table: false
            },
            net: {
                oct1: 0||null,
                oct2: 0||null,
                oct3: 0||null,
                oct4: 0||null,
            },
            auxNet: {
                oct1: 0,
                oct2: 0,
                oct3: 0,
                oct4: 0,
                mask: 0
            },
            subn: {
                nSub: 3||null,
                nSubMax: 12
            },
            wans: {
                nWan: 0||null,
                nWanMax: 30
            },
            results: [],
            resultsWans: []
        }
    },
    mounted() {
        this.initial();
    },
    methods: {
        resetSubredes() {
            this.subn = {
                nSub: 3||null,
                nSubMax: 12
            }
        },
        initial() {
            var f1 = document.getElementById("form1");
            f1.style.display = "block";
            var f2 = document.getElementById("form2");
            f2.style.display = "none";
            var wn = document.getElementById("wans");
            wn.style.display = "none";
            var tb = document.getElementById("results")
            tb.style.display = "none"
        },
        validate(index) {
            if( index === 1) {
                if (this.net.oct1 === 0 || this.net.oct1 === null || this.net.oct1 > 255 ||
                    this.net.oct2 === 0 || this.net.oct2 === null || this.net.oct2 > 255 ||
                    this.net.oct3 === 0 || this.net.oct3 === null || this.net.oct3 > 255 ||
                    this.net.oct4 === 0 || this.net.oct4 === null || this.net.oct4 > 255 ||
                    this.subn.nSub === 0 ||this.subn.nSub === null ) {
                    alert("Falta la IP prinpal o el número de subredes");
                } else {
                    if (this.forms.inWan && (this.wans.nWan === 0 || this.wans.nWan === null)) {
                        alert("Falta la cantidad de WANs");
                    } else {
                        this.ocultarForm(1);
                    }
                }
            } else if (index === 2) {
                var hostList = document.querySelectorAll("input[name=\"host[]\"]");
                var band = false;
                hostList.forEach((host) => {
                    if (host.value === "" || host.value === null || host.value === 0) {
                        band = true;
                    }
                });

                if (band) {
                    alert("Faltan datos");
                } else {
                    this.ocultarForm(3);
                }
            }
        },
        ocultarForm(index) {
            if( index === 1) {
                // Muestra el segundo formulario
                this.forms.form1 = true;
                this.forms.form2 = false;
                this.forms.table = true;
            } else if ( index === 2) {
                // Muestra el primer formulario
                this.forms.form1 = false;
                this.forms.form2 = true;
                this.forms.table = true;
            } else if( index === 3) {
                // Muestra el tabla
                this.forms.form1 = true;
                this.forms.form2 = true;
                this.forms.table = false;
            }

            var f1 = document.getElementById("form1");
            var f2 = document.getElementById("form2");
            var f3 = document.getElementById("results");

            if(this.subn.nSub <= this.subn.nSubMax) {
                if (index === 1) {
                    this.addRows();
                    setTimeout(function () {
                        f1.style.display = "none";
                        f2.style.display = "block";
                        f3.style.display = "none";
                    }, 800);
                } 
            } else {
                alert("Lo sentimos, sólo se permiten máximo 12 Subredes LAN");
            }

            if (index === 2) {
                this.resetSubredes();
                this.deleteRows();
                setTimeout(function () {
                    f1.style.display = "block";
                    f2.style.display = "none"
                    f3.style.display = "none"
                }, 800);
            }

            if (index === 3) {
                
                setTimeout(function () {
                    f1.style.display = "none";
                    f2.style.display = "none"
                    f3.style.display = "block"
                }, 800);

                this.calculate();
            }

        },
        showInputWans() {
            this.forms.inWan = !this.forms.inWan;
            var w = document.getElementById("wans");
            if (this.forms.inWan) {
                w.style.display = "block";
            } else {
                w.style.display = "none"
            }
        },
        addRows() {
            for (var i = 1; i <= this.subn.nSub; i++) {
                let con = document.getElementById("subnetlist");
                let div = document.createElement("div");
                div.classList.add("row", "px-3", "mt-3", "align-items-center");
                div.innerHTML = "<div class=\"col-7\">\
                                    <span class=\"form-control text-info\"> Subnet " + i + "</span>\
                                </div>\
                                <div class=\"col-5\">\
                                    <input type=\"number\" min=\"1\" class=\"form-control\" name=\"host[]\" required>\
                                </div>";
                con.appendChild(div);
            }
        },
        deleteRows() {
            let con = document.getElementById("subnetlist");
            while (con.firstChild) {
                con.removeChild(con.firstChild);
            }
        },
        calculate() {
            var hostList = document.querySelectorAll("input[name=\"host[]\"]");
            var hosts = 0;
            var cont = 1;
            var main = "";

            main = "RP => " + this.net.oct1 + "." + this.net.oct2 + "." + this.net.oct3 + "." + this.net.oct4;
            console.log(main);
            
            hostList.forEach((host ) => {
                var subr = "";
                var prim = "";
                var ulti = "";
                var gate = "";
                var broa = "";
                var o1, o2, o3, o4;
                let band = false;
                let i = 2;
                // Calcula los hosts que se ajustan
                do {
                    if (Math.pow(2, i) > host.value) {
                        hosts = Math.pow(2, i) - 2;
                        band = true;
                    } else {
                        i++;
                    }
                } while (band == false);

                this.auxNet.mask = 32-i;

                console.log(cont + " - " + host.value + ' : ' + hosts + ' : ' + this.auxNet.mask);

                // Calcular Principal
                this.auxNet.oct1 = Number(this.net.oct1);
                this.auxNet.oct2 = Number(this.net.oct2);
                this.auxNet.oct3 = Number(this.net.oct3);
                this.auxNet.oct4 = Number(this.net.oct4);
                subr = this.auxNet.oct1 + "." + this.auxNet.oct2 + "." + this.auxNet.oct3 + "." + this.auxNet.oct4 + "/" + (32-i);
                // console.log("R => " + this.auxNet.oct1 + "." + this.auxNet.oct2 + "." + this.auxNet.oct3 + "." + this.auxNet.oct4 + "/" + (32-i));

                // Calcular Primera
                this.auxNet.oct1 = Number(this.net.oct1);
                this.auxNet.oct2 = Number(this.net.oct2);
                if((this.net.oct4 + 1) > 255) {
                    this.auxNet.oct3 = Number(this.net.oct3) + 1;
                    if( (hosts % 255) === 1) {
                        this.auxNet.oct4 = (hosts % 255) + 1;
                    }
                    
                }else {
                    this.auxNet.oct3 = Number(this.net.oct3);
                    this.auxNet.oct4 = Number(this.net.oct4) + 1;
                }
                prim = this.auxNet.oct1 + "." + this.auxNet.oct2 + "." + this.auxNet.oct3 + "." + this.auxNet.oct4;
                // console.log("P => " + this.auxNet.oct1 + "." + this.auxNet.oct2 + "." + this.auxNet.oct3 + "." + this.auxNet.oct4);

                // Calcular Última
                o3 = this.auxNet.oct3;
                o4 = this.auxNet.oct4;
                this.auxNet.oct1 = Number(this.net.oct1);
                this.auxNet.oct2 = Number(this.net.oct2);
                if((o4 + hosts) <= 255) {
                    this.auxNet.oct3 = Number(this.auxNet.oct3);
                    this.auxNet.oct4 = Number(this.auxNet.oct4) + hosts - 2;
                } else {
                    this.auxNet.oct3 = Number(this.auxNet.oct3) + Math.floor(hosts / 255);
                    this.auxNet.oct4 = Number(this.auxNet.oct4) + (hosts % 255) - 2;
                }
                ulti = this.auxNet.oct1 + "." + this.auxNet.oct2 + "." + this.auxNet.oct3 + "." + this.auxNet.oct4;
                // console.log("U => " + this.auxNet.oct1 + "." + this.auxNet.oct2 + "." + this.auxNet.oct3 + "." + this.auxNet.oct4);

                // Calcular Gateway
                this.auxNet.oct1 = Number(this.auxNet.oct1);
                this.auxNet.oct2 = Number(this.auxNet.oct2);
                this.auxNet.oct3 = Number(this.auxNet.oct3);
                this.auxNet.oct4 = Number(this.auxNet.oct4) + 1;
                gate = this.auxNet.oct1 + "." + this.auxNet.oct2 + "." + this.auxNet.oct3 + "." + this.auxNet.oct4;
                // console.log("G => " + this.auxNet.oct1 + "." + this.auxNet.oct2 + "." + this.auxNet.oct3 + "." + this.auxNet.oct4);

                // Calcular Broadcast
                this.auxNet.oct1 = Number(this.auxNet.oct1);
                this.auxNet.oct2 = Number(this.auxNet.oct2);
                this.auxNet.oct3 = Number(this.auxNet.oct3);
                this.auxNet.oct4 = Number(this.auxNet.oct4) + 1;
                broa = this.auxNet.oct1 + "." + this.auxNet.oct2 + "." + this.auxNet.oct3 + "." + this.auxNet.oct4
                // console.log("B => " + this.auxNet.oct1 + "." + this.auxNet.oct2 + "." + this.auxNet.oct3 + "." + this.auxNet.oct4);
                // console.log("---------------------------");

                this.results.push({
                    i: cont,
                    r: subr,
                    p: prim,
                    u: ulti,
                    g: gate,
                    b: broa
                });

                // Deja lista la nueva Subed
                this.net.oct1 = this.auxNet.oct1;
                this.net.oct2 = this.auxNet.oct2;
                if((Number(this.auxNet.oct4) + 1) > 255) {
                    this.net.oct3 = Number(this.auxNet.oct3) + 1;
                    this.net.oct4 = 0;
                } else {
                    this.net.oct3 = this.auxNet.oct3;
                    this.net.oct4 = this.auxNet.oct4 + 1;
                }
                
                cont++; 
            });

            if(this.forms.inWan) {
                this.calculateWans();
            }
            // console.log(this.results);
        },
        calculateWans() {
            console.log("Hola: " + this.wans.nWan);
            var hosts = 0;
            console.log("<<<<<<<<<<<<<<<<<<<<<<<<<<");
            for(var i=1;i<=this.wans.nWan;i++) {
                var subr = "";
                var prim = "";
                var ulti = "";
                var broa = "";
                var o1, o2, o3, o4;
                let band = false;
                // Calcular Principal
                this.auxNet.oct4 = Number(this.auxNet.oct4) + 1;
                subr = this.auxNet.oct1 + "." + this.auxNet.oct2 + "." + this.auxNet.oct3 + "." + this.auxNet.oct4 + "/" + 30;
                // console.log("R => " + this.auxNet.oct1 + "." + this.auxNet.oct2 + "." + this.auxNet.oct3 + "." + this.auxNet.oct4 + "/" + 30);

                // Calcular Primera
                this.auxNet.oct1 = Number(this.auxNet.oct1);
                this.auxNet.oct2 = Number(this.auxNet.oct2);
                if((this.auxNet.oct4 + 1) > 255) {
                    this.auxNet.oct3 = Number(this.auxNet.oct3) + 1;
                    if( (hosts % 255) === 1) {
                        this.auxNet.oct4 = (hosts % 255) + 1;
                    }
                }else {
                    this.auxNet.oct3 = Number(this.auxNet.oct3);
                    this.auxNet.oct4 = Number(this.auxNet.oct4) + 1;
                }
                prim = this.auxNet.oct1 + "." + this.auxNet.oct2 + "." + this.auxNet.oct3 + "." + this.auxNet.oct4;
                // console.log("P => " + this.auxNet.oct1 + "." + this.auxNet.oct2 + "." + this.auxNet.oct3 + "." + this.auxNet.oct4);

                // Calcular Última
                o3 = this.auxNet.oct3;
                o4 = this.auxNet.oct4;
                this.auxNet.oct1 = Number(this.auxNet.oct1);
                this.auxNet.oct2 = Number(this.auxNet.oct2);
                if((o4 + 1) <= 255) {
                    this.auxNet.oct3 = Number(this.auxNet.oct3);
                    this.auxNet.oct4 = Number(this.auxNet.oct4) + 1;
                } else {
                    this.auxNet.oct3 = Number(this.auxNet.oct3) + Math.floor(hosts / 255);
                    this.auxNet.oct4 = Number(this.auxNet.oct4) + 1;
                }
                ulti = this.auxNet.oct1 + "." + this.auxNet.oct2 + "." + this.auxNet.oct3 + "." + this.auxNet.oct4;
                // console.log("U => " + this.auxNet.oct1 + "." + this.auxNet.oct2 + "." + this.auxNet.oct3 + "." + this.auxNet.oct4);

                // Calcular Broadcast
                this.auxNet.oct1 = Number(this.auxNet.oct1);
                this.auxNet.oct2 = Number(this.auxNet.oct2);
                this.auxNet.oct3 = Number(this.auxNet.oct3);
                this.auxNet.oct4 = Number(this.auxNet.oct4) + 1;
                broa = this.auxNet.oct1 + "." + this.auxNet.oct2 + "." + this.auxNet.oct3 + "." + this.auxNet.oct4;
                // console.log("B => " + this.auxNet.oct1 + "." + this.auxNet.oct2 + "." + this.auxNet.oct3 + "." + this.auxNet.oct4);
                // console.log("---------------------------");

                this.resultsWans.push({
                    i: i,
                    r: subr,
                    p: prim,
                    u: ulti,
                    b: broa
                });

                // Deja lista la nueva Subed
                this.net.oct1 = this.auxNet.oct1;
                this.net.oct2 = this.auxNet.oct2;
                if((Number(this.auxNet.oct4) + 1) > 255) {
                    this.net.oct3 = Number(this.auxNet.oct3) + 1;
                    this.net.oct4 = 0;
                } else {
                    this.net.oct3 = this.auxNet.oct3;
                    this.net.oct4 = this.auxNet.oct4 + 1;
                }
            }
        }
    }
})
</script>
