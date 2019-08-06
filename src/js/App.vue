<template>
    <div class="wrapper">
        <h1>Generátor Email Podpisu</h1>
        <canvas width="450" height="250" ref="cardCanvas"/>
        <div class="inputs">
            <div class="form-row">
                <label for="name">Jméno a příjmení</label>
                <input type="text" id="name" v-model="name" placeholder="Jan Novák"/>
            </div>
            <div class="form-row">
                <label for="position">Pozice</label>
                <input type="text" id="position" v-model="position" placeholder="Manager výroby"/>
            </div>
            <div class="form-row">
                <label for="phone1">Telefon 1</label>
                <input type="tel" id="phone1" v-model="phone1" placeholder="+420 123 456 798"/>
            </div>
            <div class="form-row">
                <label for="phone2">Telefon 2</label>
                <input type="tel" id="phone2" v-model="phone2" placeholder="+420 123 456 798"/>
            </div>
            <div class="form-row">
                <label for="email">Email</label>
                <input type="email" id="email" v-model="email" placeholder="jan.novak@envi-pur.cz"/>
            </div>
            <div class="form-row">
                <button @click="save()">Stáhnout</button>
            </div>
        </div>
    </div>
</template>

<script>
    import template from '../img/template.jpg'

    export default {
        name: 'App',
        data: function () {
            return {
                // name: 'Ing. Jméno Příjmení',
                // position: 'Obchodní ředitel',
                // phone1: '+420 381 203 218',
                // phone2: '+420 123 456 789',
                // email: 'prijmeni@envi-pur.cz',
                name: '',
                position: '',
                email: '',
                phone1: '',
                phone2: ''
            }
        },
        computed: {
            canvas() {
                return this.$refs.cardCanvas
            },
            canvasContext() {
                return this.canvas.getContext("2d")
            },
            changingData() {
                return this.name + this.position + this.email + this.phone1 + this.phone2
            }
        },
        watch: {
            changingData: function () {
                this.paint()
            }
        },
        mounted() {
            this.paint()
        },
        methods: {
            save() {
                const a = document.createElement('a')
                a.href = this.canvas.toDataURL("image/jpeg", 0.75)
                a.download = `${this.email.replace(/[^\x00-\x7F]/g, "")}${new Date().valueOf()}.jpg`
                document.body.appendChild(a)
                a.click()
            },
            paint() {
                const ctx = this.canvasContext
                const img = new Image()
                img.src = template
                img.onload = () => {
                    ctx.drawImage(img, 0, 0);

                    ctx.font = 'bold 17px Tahoma'
                    ctx.fillStyle = '#003187'
                    ctx.fillText(this.name, 10, 79);

                    ctx.font = '15px Tahoma'
                    ctx.fillStyle = '#2f2e2d'
                    ctx.fillText(this.position, 10, 99);

                    ctx.font = '14px Tahoma'
                    ctx.fillStyle = '#2f2e2d'
                    ctx.fillText(this.phone1, 36, 131);

                    ctx.font = '14px Tahoma'
                    ctx.fillStyle = '#2f2e2d'
                    ctx.fillText(this.phone2, 204, 131);

                    ctx.font = '14px Tahoma'
                    ctx.fillStyle = '#2f2e2d'
                    ctx.fillText(this.email, 55, 148);
                }
            }
        }
    }
</script>

<style lang="scss">
    body {
        font-family: Tahoma;
        background: #e8f0fe;

        .wrapper {
            text-align: center;
            color: #003187;
        }

        canvas {
            /*border: 1px solid black;*/
        }

        .inputs {
            .form-row {
                label {
                    display: block;
                    text-align: center;
                    font-size: 16px;
                    margin: 10px 0 5px;
                    font-weight: bold;
                }
                input {
                    margin: 0 auto;
                    display: block;
                    text-align: center;
                    font-size: 14px;
                    color: #003187;
                    border: 1px solid #003187;
                    width: 300px;
                    min-height: 25px;
                    border-radius: 5px;
                    padding: 0 0;
                    line-height: 25px;
                    background: #f5f9ff;
                }
                button {
                    width: 300px;
                    height: 35px;
                    border-radius: 5px;
                    margin: 15px 0;
                    background: #003187;
                    color: #ffffff;
                    border: 0;
                    cursor: pointer;
                    transition: .2s ease;
                    font-weight: bold;

                    &:hover {
                        background: #325487;
                    }
                }
            }
        }
    }
</style>
