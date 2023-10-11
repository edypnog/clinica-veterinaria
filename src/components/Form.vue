<script setup>
import { reactive, computed } from 'vue';
import AlertMsg from './AlertMsg.vue'

const alert = reactive({
    msg: '',
    type: ''
});

const props = defineProps({
    id: {
        type: [String, null],
        required: true,
    },
    nome: {
        type: String,
        required: true,
    },
    dono: {
        type: String,
        required: true,
    },
    email: {
        type: String,
        required: true,
    },
    data: {
        type: String,
        required: true,
    },
    sintomas: {
        type: String,
        required: true,
    },
});

const emit = defineEmits(['update:nome', 'update:dono', 'update:email', 'update:data', 'update:sintomas', 'salvar-paciente']);

const validar = () => {
    if(Object.values(props).includes('')) {
        alert.msg = 'Todos os campos são obrigatórios';
        alert.type = 'error';
        return;
    }

    emit('salvar-paciente');

    alert.msg = 'Paciente adicionado com sucesso';
    alert.type = 'success';

    setTimeout(() => {
        Object.assign(alert, {
            msg: '',
            type: ''
        })
    }, 3000);
}

const editando = computed(() => {
    return props.id
});

</script>

<template>
    <div class="md:w-1/2">
        <h2 class="text-center text-3xl font-black">Preencha os campos</h2>
        <p class="text-lg mt-5 text-center mb-10">
            Adicione os pacientes para
            <span class="text-indigo-600 font-bold">a lista.</span>
        </p>

        <AlertMsg
            v-if="alert.msg"
            :alert="alert"
        />

        <form 
        @submit.prevent="validar"
        class="
            bg-white
            shadow-md
            rounded-lg
            py-5
            px-5
            my-5
        ">
            <div class="mb-5">
                <label 
                    for="pet"
                    class="block text-gray-700 uppercase font-bold"
                >
                    Nome do pet
                </label>
                <input 
                    type="text"
                    id="pet"
                    placeholder="Nome do pet"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    :value="nome"
                    @input="$emit('update:nome', $event.target.value)"
                />
            </div>

            <div class="mb-5">
                <label 
                    for="dono"
                    class="block text-gray-700 uppercase font-bold"
                >
                    Nome do dono
                </label>
                <input 
                    type="text" 
                    id="dono"
                    placeholder="Nome do dono"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    :value="dono"
                    @input="$emit('update:dono', $event.target.value)"
                />
            </div>

            <div class="mb-5">
                <label 
                    for="email"
                    class="block text-gray-700 uppercase font-bold"
                >
                    E-mail
                </label>
                <input 
                    type="email"
                    id="email"
                    placeholder="E-mail"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    :value="email"
                    @input="$emit('update:email', $event.target.value)"
                />
            </div>

            <div class="mb-5">
                <label 
                    for="data"
                    class="block text-gray-700 uppercase font-bold"
                >
                    Data de alta
                </label>
                <input 
                    type="date"
                    id="data"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    :value="data"
                    @input="$emit('update:data', $event.target.value)"
                />
            </div>

            <div class="mb-5">
                <label 
                    for="sintomas"
                    class="block text-gray-700 uppercase font-bold"
                >
                    Sintomas
                </label>
                <textarea 
                    name="sintomas" 
                    id="sintomas" 
                    placeholder="Descreva os sintomas" 
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md h-40"
                    :value="sintomas"
                    @input="$emit('update:sintomas', $event.target.value)"
                ></textarea>
            </div>

            <input 
            type="submit" 
            :value="editando ? 'Salvar' : 'Registrar Paciente'"
            class="bg-indigo-700 w-full p-3 uppercase text-white font-bold hover:bg-indigo-950 cursor-pointer transition-all rounded-md" />
        </form>
    </div>
</template>

