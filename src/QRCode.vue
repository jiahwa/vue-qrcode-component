
<template>
    <div></div>
</template>

<script>
    import {ref, watch, onMounted, getCurrentInstance} from 'vue'
    import QRCode from '@keeex/qrcodejs-kx'
    export default {

        props: {
            text: {type: String, required: true},
            size: {type: Number, required: false, default: 256},
            color: {type: String, required: false, default: '#000'},
            bgColor: {type: String, required: false, default: '#FFF'},
            errorLevel: {
                type: String, 
                // c: value => value === 'L' || value === 'M' || value === 'Q' || value === 'H'
                required: false, 
                default: 'H'
            }
        },

        setup(props) {
            const qrCode = ref({})
            const that = getCurrentInstance()

            onMounted(() => {
                qrCode.value = new QRCode(that.el, {
                    text: props.text,
                    width: props.size,
                    height: props.size,
                    colorDark : props.color,
                    colorLight : props.bgColor,
                    correctLevel : QRCode.CorrectLevel[props.errorLevel]
                });
            })
            
            const clear = () => qrCode.value.clear()
            const makeCode = text => qrCode.value.makeCode(text);

            watch(() => props.text,
            val => {
                clear();
                makeCode(val);
            })

            return{
                qrCode,
                clear,
                makeCode
            }
        }
    }
</script>