<template>
  <div>
    <Head title="Novo cliente" />
    <h1 class="mb-8 text-3xl font-bold">
      <Link class="text-indigo-400 hover:text-indigo-600" href="/contacts">Clientes</Link>
      <span class="text-indigo-400 font-medium">/</span> Novo
    </h1>
    <div class="max-w-full bg-white rounded-md shadow overflow-hidden">
      <form @submit.prevent="store">  
        <div class="flex flex-wrap -mb-10 -mr-6 p-8">
          
          <select-input v-model="form.cliente_tipo" :error="form.errors.cliente_tipo" id="categoria"  onchange="mudaCategoria()"  class="pb-6 pr-2  w-full lg:w-1/3" label="Pessoa Física ou Jurídica ou Estrangeiro?* ">
            <option value="fisica">Física</option>
            <option value="juridica">Jurídica</option>
            <option value="estrangeiro">Estrangeiro</option>
          </select-input>
          <select-input v-model="form.contribuinte" :error="form.errors.contribuinte" class="pb-6 pr-2 w-full lg:w-1/3" label="Contribuinte">
            <option value="consumidor_final">Consumidor final</option>
            <option value="contribuinte">Contribuinte</option>
          </select-input>
          <text-input v-model="form.cpf_cnpj" :error="form.errors.cpf_cnpj" class="pb-6 pr-2 w-full lg:w-1/3" placeholder="000.000.000-00" label="CPF" />
          <text-input v-model="form.first_name" :error="form.errors.first_name" class="pb-6 pr-2 w-full lg:w-1/4" label="Nome*" />
          <text-input v-model="form.last_name" :error="form.errors.last_name" class="pb-6 pr-2 w-full lg:w-1/4" label="Sobrenome*" />
          <text-input v-model="form.email" :error="form.errors.email" class="pb-6 pr-2 w-full lg:w-1/3" label="Email" />
          <!--<select-input v-model="form.organization_id" :error="form.errors.organization_id" class="pb-6 pr-2 w-full lg:w-1/2" label="Organization">
            <option :value="null" />
            <option v-for="organization in organizations" :key="organization.id" :value="organization.id">{{ organization.name }}</option>
          </select-input>-->
          <text-input v-model="form.telefone" :error="form.errors.telefone" class="pb-6 pr-2 w-full lg:w-1/6" label="Telefone" />
          <text-input v-model="form.inscr_suframa" :error="form.errors.inscr_suframa" class="pb-6 pr-2 w-full lg:w-1/6" label="Insc. Suframa" />
          <input type="checkbox" id="checkbox" v-model="form.isento"><label class="mt-9 pb-6 pl-1 pr-4">Isento de I.E.</label>
          <text-input v-model="form.inscr_estadual" :error="form.errors.inscr_estadual" class="pb-6 pr-2 w-full lg:w-1/6" label="Insc. Estadual" />
          <text-input v-model="form.inscr_municipal" :error="form.errors.inscr_municipal" class="pb-6 pr-2 w-full lg:w-1/6" label="Insc. Municipal" />
          <text-input v-model="form.organization" :error="form.errors.inscr_municipal" class="pb-6 pr-2 w-full lg:w-1/6" label="Insc. Municipal" />

          
          <h2 class="w-full pb-6 font-bold">Endereço:</h2>
          <text-input v-model="form.postal_code" :error="form.errors.postal_code" class="pb-6 pr-2 w-full lg:w-1/3" placeholder="00000-000" label="CEP" />
          <text-input v-model="form.address" :error="form.errors.address" class="pb-6 pr-2  lg:w-1/3" label="Logradouro" />
          <text-input v-model="form.complemento" :error="form.errors.complemento" class="pb-6 pr-2 w-full lg:w-1/6" label="Complemento" />
          <text-input v-model="form.numero" :error="form.errors.numero" class="pb-6 pr-2 w-full lg:w-1/6" label="Número" />
          <text-input v-model="form.bairro" :error="form.errors.bairro" class="pb-6 pr-2 w-full lg:w-1/3" label="Bairro" />
          <text-input v-model="form.city" :error="form.errors.city" class="pb-6 pr-2 w-full lg:w-1/3" label="Cidade" />
          <text-input v-model="form.region" :error="form.errors.region" class="pb-6 pr-2 w-full lg:w-1/6" label="Estado" />
          <select-input v-model="form.pais" :error="form.errors.pais" class="pb-6 pr-2 w-full lg:w-1/6" label="Pais">
            <option value="CA">Canada</option>
            <option value="US">United States</option>
          </select-input>
          <h2 class="w-full pb-6 font-bold">Outros:</h2>
          <text-input v-model="form.obs" :error="form.errors.obs" class="pb-6 pr-2 w-full" label="Observações" />

        </div>
        <div class="flex items-center justify-end px-8 py-4 bg-gray-50 border-t border-gray-100">
          <loading-button :loading="form.processing" class="btn-indigo" type="submit">Novo cliente</loading-button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import { Head, Link } from '@inertiajs/inertia-vue3'
import Layout from '@/Shared/Layout'
import TextInput from '@/Shared/TextInput'
import SelectInput from '@/Shared/SelectInput'
import LoadingButton from '@/Shared/LoadingButton'

export default {
  components: {
    Head,
    Link,
    LoadingButton,
    SelectInput,
    TextInput,
  },
  layout: Layout,
  props: {
  },
  remember: 'form',
  data() {
    return {
      form: this.$inertia.form({
        first_name: '',
        last_name: '',
        organization_id: null,
        email: '',
        phone: '',
        address: '',
        city: '',
        region: '',
        country: '',
        postal_code: '',

        cliente_tipo: '',
        contribuinte: '',
        cpf_cnpj: '',
        inscr_estadual:'',
        inscr_municipal:'',
        inscr_suframa:'',
        numero:'',
        bairro:'',
    
      }),
    }
  },
  methods: {
    store() {
      this.form.post('/contacts')
    },
  },

}

function mudaCategoria() {
        if (document.getElementById("categoria").value === "fisica") {
            $('#cpf_cnpj').attr("placeholder", "Insira o CPF").attr("maxlength", "14").attr("max", "14").attr("min", "14").siblings('label').html('CPF:').parent().show();
          
           
        } else if (document.getElementById("categoria").value === "juridica") {
            $('#nome').attr("placeholder", "Insira o nome fantasia").siblings('label').html('Nome Fantasia:');
            $('#cpf_cnpj').attr("placeholder", "Insira o CNPJ").attr("maxlength", "18").attr("max", "18").attr("min", "18").siblings('label').html('CNPJ:').parent().show();
            $('#aux_razao').show();
            $("#cpf_cnpj").keyup();
            
          
        } else {
            $('#nome').attr("placeholder", "Insira o nome").siblings('label').html('Nome*:');
            $('#pais, #cpf_cnpj_e').parent().show();
            $('#aux_razao').hide();
            $('#inc_estadual, #cpf_cnpj').val('').parent().hide();
            $('#cpf_cnpj')[0].setCustomValidity('');
            $('#inc_suframa').val('').parent().hide();
            $('#inc_municipal').val('').parent().hide();
            $('#ufE, #localidadeE').val('').parent().show();
            $('#UF').append(new Option('EX', '999999', false, false)).val('999999').parent().hide();
            $('#localidade').append(new Option('999999', '999999', false, false)).val('999999').parent().hide();
            stampEstrangeiro = true;
        }
    }
</script>
