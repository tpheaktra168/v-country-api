<template>
  <v-card>
      <v-card-title>
          Country Catalog Implementation
          <v-spacer></v-spacer>
          <v-text-field v-model="search" append-icon="mdi-magnify" label="Search" single-line hide-details></v-text-field>
      </v-card-title>
      <v-data-table :headers="headers" :items="filteredData" :items-per-page="25"  @click:row="fShowDetails">
         
        <template v-slot:[`item.flags`]="{ item }">
          <a href=""  @click:row="fShowDetails" data-toggle="modal" data-target="#exampleModal" >
            <img :src="item.flags.png" alt="Item Image" width="50px"/>
          </a>   
          </template>
          <template v-slot:[`item.con_name`]="{ item }">
              <span>{{ item.name.official }}</span>
          </template>
          <template v-slot:[`item.native_name`]="{ item }">
              <span>{{ item.name.nativeName }}</span>
          </template>

          <template v-slot:[`item.cca2`]="{ item }">
              <span>{{ item.cca2 }} </span>
          </template>
          
          <template v-slot:[`item.cca3`]="{ item }">
              <span>{{ item.cca3 }}</span>
          </template>
          
          <template v-slot:[`item.alt_pellings`]="{ item }">
              <span>{{ item.altSpellings[0]}}</span>
          </template>

          <template v-slot:[`item.idd`]="{ item }">
              <span>{{ item.idd.root}}</span>
          </template>
          

      </v-data-table>
    
      <!-- Modal -->
      <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
        <div class="modal-dialog">
          <div class="modal-content">
            <div class="modal-header">
              <h4 class="modal-title" id="exampleModalLabel">Detail Information</h4>
              <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                <span aria-hidden="true">&times;</span>
              </button>
            </div>
            <div class="modal-body">
               <ul v-if="showDetail">
                  <li>
                    Country Name: {{ showDetail.name.official }} : <img style="width: 30px;" :src="showDetail.flags.png"/>
                  </li>
                  <li>
                    CCA2: {{ showDetail.cca2 }} : CCA3 {{ showDetail.cca3 }}
                  </li>
                  <li>
                    altSpellings: {{ showDetail.altSpellings[0] }}
                  </li>
                  <li>
                   IDD: {{ showDetail.idd.root }}
                  </li>
                  <li>
                    languages: {{ showDetail.languages}}
                  </li>
               </ul> 
            </div> 
          </div>
        </div>
      </div>

  </v-card>
</template>

<script>
 import axios from 'axios';

  export default {
    data () {
      return {
        search: '',
        
        headers: [
          { text: 'Flags',                      value: 'flags',      sortable: false,},
          { text: 'Country Name',               value: 'name.official',                },
          { text: 'CCA2',   value: 'cca2',      sortable: false, },
          { text: 'CCA3',   value: 'cca3',      sortable: false, },
          { text: 'Native Country Name',        value: 'native_name', sortable: false, },
          { text: 'AltSpellings',   value: 'alt_pellings',sortable: false, },
          { text: 'IDD',      value: 'idd' ,        sortable: false, },
        ],
        data:[],
        showDetail:'',
        dialog:false
      }
    },
    created(){
        this.funGetData();
    },
    computed: {
      filteredData() {
        return this.data.filter((item) => {
          return item.name.official.toLowerCase().includes(this.search.toLowerCase());
        });
      },
    },
    methods:{
      funGetData() {
          axios
              .get("https://restcountries.com/v3.1/all",{
                // params:{
                //   con_name:this.search,
                //   per_page:25,
                // }
              })
              .then((res) => {
                  this.data = res.data;
              })
              .catch((error) => {
                  console.log(error);
              });
      },
      fShowDetails(item) {
        this.dialog = true;
        this.showDetail = item;
      
      },
    }
  }
</script>