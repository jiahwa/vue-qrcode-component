
<template>
    <div ref="qrRoot"></div>
</template>

<script>
    import {ref, watch, onMounted} from 'vue'
    import QRCode from '@keeex/qrcodejs-kx'
    export default {

        props: {
            text: {type: String, required: true},
            size: {type: Number, required: false, default: 256},
            color: {type: String, required: false, default: '#000'},
            bgColor: {type: String, required: false, default: '#FFF'},
            errorLevel: {
                type: String,
                validator(value) {
                    return value === 'L' || value === 'M' || value === 'Q' || value === 'H'
                },
                required: false, 
                default: 'H'
            }
        },

        setup(props) {
            const qrCode = ref({})
            const qrRoot = ref(null)
            const clear = () => qrCode.value.clear()
            const makeCode = text => qrCode.value.makeCode(text);

            watch(() => props.text,
            val => {
                clear();
                makeCode(val);
            })

            onMounted(() => {
                qrCode.value = new QRCode(qrRoot.value, {
                    text: props.text,
                    width: props.size,
                    height: props.size,
                    colorDark : props.color,
                    colorLight : props.bgColor,
                    correctLevel : QRCode.CorrectLevel[props.errorLevel]
                });
            })

            return{
                qrCode,
                qrRoot,
                clear,
                makeCode
            }
        }
    }
</script>