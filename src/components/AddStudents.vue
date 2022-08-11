<template>
  <div class="flex">
    <v-data-table
      v-model="selected"
      :single-select="singleSelect"
      item-key="sıra"
      show-select
      :headers="headers"
      :items="students"
      sort-by="sıra"
      :footer-props="{
        itemsPerPageText: 'Satır sayısı : '
      }"
    >
      <template v-slot:top>
        
        <v-toolbar
          flat
        >
          <v-switch
          v-model="singleSelect"
          label="Tekli seçim"
          class="ml-2 mt-3"
          />
         
          <v-spacer></v-spacer>

          <v-dialog
            v-model="dialogImportExcel"
            max-width="1000px"
          >
            <template v-slot:activator="{ on, attrs }">
              <v-btn
                class="mr-4 mb-2"
                color="primary"
                dark
                v-bind="attrs"
                v-on="on"
                
              >
                TOPLU ÖĞRENCİ EKLE
              </v-btn>
            </template>

            <v-card>
              <v-card-title>
                <span class="text-h5 mr-4">Toplu Öğrenci Ekle</span>
                <sub class="text--secondary">Öğrencilerin bilgilerini aşağıdaki şablona doldurup sisteme yüklemelisiniz</sub>
              </v-card-title>

              <v-card-text>
                <v-container>
                  <div class="add_pattern">
                    <span >Toplu öğrenci ekleme şablonunu indirmek için </span>  <a href="../../public/add_pattern.XLS" download="deneme.XLS">tıklayınız</a>
                  </div>
                  <div class="add_students">
                    <a
                      href="javascript:;"
                      class="add_students_button"
                    >
                      <input type="file" class="my_input mt-8" @change="importExcel" id="upload" />
                    </a>

                  </div>  
                </v-container>
              </v-card-text>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn
                  color="blue darken-1"
                  text
                  @click="closeDialog"
                >
                  Kapat
                </v-btn>
              </v-card-actions>
            </v-card>

            

          </v-dialog>

          <v-dialog
            v-model="dialogAddStudent"
            max-width="1000px"
          >
            <template v-slot:activator="{ on, attrs }">
              <v-btn
                color="primary"
                dark
                class="mb-2"
                v-bind="attrs"
                v-on="on"
              >
                YENİ ÖĞRENCİ
              </v-btn>
            </template>

            <v-card>
              <v-card-title>
                <span class="text-h5 text mr-4">YENİ ÖĞRENCİ</span>
                <sub class="text--secondary">Öğrenci bilgilerini doldurunuz</sub>
              </v-card-title>

              <v-card-text>
                <v-container>
                  <v-row>
                    <v-col
                      cols="12"
                      sm="6"
                      md="4"
                    >
                      <v-text-field
                        class="required_field"
                        color="##F44336"
                        v-model="newStudent.ad"
                        label="Adı*"
                        required
                        :rules="['Required']"
                      ></v-text-field>
                    </v-col>

                    <v-col
                      cols="12"
                      sm="6"
                      md="4"
                    >
                      <v-text-field
                        class="required_field"
                        color="##F44336"
                        v-model="newStudent.soyad"
                        label="Soy Adı*"
                      ></v-text-field>
                    </v-col>

                    <v-col
                      cols="12"
                      sm="6"
                      md="4"
                    >
                      <v-text-field
                        class="required_field"
                        color="##F44336"
                        v-model="newStudent.ogrenciNo"
                        label="Okul Numarası*"
                      ></v-text-field>
                    </v-col>

                    <v-col
                      cols="12"
                      sm="6"
                      md="4"
                    >
                      <v-text-field
                        class="required_field"
                        color="##F44336"
                        v-model="newStudent.sınıf"
                        label="Sınıfı*"
                      ></v-text-field>
                    </v-col>

                    <v-col
                      cols="12"
                      sm="6"
                      md="4"
                    >
                      <v-text-field
                        class="required_field"
                        color="##F44336"
                        v-model="newStudent.sube"
                        label="Şubesi*"
                      ></v-text-field>
                    </v-col>


                    <v-col
                      cols="12"
                      sm="6"
                      md="4"
                    >
                      <v-text-field
                        v-model="newStudent.cinsiyet"
                        label="Cinsiyeti"
                      ></v-text-field>
                    </v-col>

                    <v-col
                      cols="12"
                      sm="6"
                      md="4"
                    >
                      <v-text-field
                        v-model="newStudent.tcno"
                        label="Kimlik Numarası"
                      ></v-text-field>
                    </v-col>

                    <v-col
                      cols="12"
                      sm="6"
                      md="4"
                    >
                      <v-text-field
                        v-model="newStudent.ogrenciTel"
                        label="Tel No"
                      ></v-text-field>
                    </v-col>

                    <v-col
                      cols="12"
                      sm="6"
                      md="4"
                    >
                      <v-text-field
                        v-model="newStudent.anneTel"
                        label="Anne Tel No"
                      ></v-text-field>
                    </v-col>

                    <v-col
                      cols="12"
                      sm="6"
                      md="4"
                    >
                      <v-text-field
                        v-model="newStudent.babaTel"
                        label="Baba Tel No"
                      ></v-text-field>
                    </v-col>

                    <v-col
                      cols="12"
                      sm="6"
                      md="4"
                    >
                      <v-text-field
                        v-model="newStudent.ogrenciMail"
                        label="Mail Adresi"
                      ></v-text-field>
                    </v-col>

                  </v-row>
                </v-container>
              </v-card-text>

              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn
                  color="blue darken-1"
                  text
                  @click="closeDialog"
                >
                  KAPAT
                </v-btn>
                <v-btn
                  color="blue darken-1"
                  text
                  @click="save"
                >
                  EKLE
                </v-btn>
              </v-card-actions>
            </v-card>

          </v-dialog>
          
        </v-toolbar>
 
      </template>

      <template v-slot:[`item.actions`]="{ item }">
        <v-icon
          small
          class="pl-3"
          @click="editItem(item)"
        >
          mdi-pencil
        </v-icon>
      
      </template>
      
      <template v-slot:[`footer.prepend`]>
        
        <v-dialog v-model="dialogDelete" max-width="1000px">        
            <template v-slot:activator="{ on, attrs }">
                <v-btn
                  class="ml-2 mb-2"
                  color="primary"
                  dark
                  v-bind="attrs"
                  v-on="on"
                >
                  SİL
                </v-btn>
            </template>

            <v-card>
              <v-card-title class="text-h5">Seçilen öğrencileri kaldırmak istediğinizden emin misiniz?</v-card-title>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" text @click="closeDialog">ÇIKIŞ</v-btn>
                <v-btn color="blue darken-1" text @click="deleteStudents">SİL</v-btn>
                <v-spacer></v-spacer>
              </v-card-actions>
            </v-card>
        </v-dialog>

      </template>

      <template>
        <v-dialog v-model="dialogComplete">
           <v-card>
              <v-card-title class="text-h5">{{dialogMessage}}</v-card-title>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" text @click="closeDialog">TAMAM</v-btn>
                <v-spacer></v-spacer>
              </v-card-actions>
            </v-card>
        </v-dialog>
      </template>

    </v-data-table>
    
  </div>
</template>

<script>
  import xlsx from "xlsx";

  export default {

    data: () => ({
        dialogAddStudent: false,
        dialogImportExcel: false,
        dialogDelete: false,
        dialogComplete: false,
        singleSelect: false,
        selected: [],
        dialogMessage: "",
        headers: [
            { text: "Sıra", value: "sıra" },
            { text: "Sınıf", value: "sınıf" },
            { text: "Şube", value: "sube" },
            { text: "Okul Numarası", value: "ogrenciNo" },
            { text: "Ad", value: "ad" },
            { text: "Soyad", value: "soyad" },
            { text: "Cinsiyet", value: "cinsiyet" },
            { text: "Kimlik Numarası", value: "tcno" },
            { text: "Anne Tel", value: "anneTel" },
            { text: "Baba Tel", value: "babaTel" },
            { text: "Öğrenci Tel", value: "ogrenciTel" },
            { text: "Öğrenci Mail", value: "ogrenciMail" },
            { text: 'Düzenle', value: 'actions', sortable: false },
        ],
       
        students: [],
        editedIndex: -1,
        newStudent: {
            sıra:"",
            sınıf:"",
            sube:"",
            ogrenciNo:"",
            ad:"",
            soyad:"",
            cinsiyet:"",
            tcno:"",
            anneTel:"",
            babaTel:"",
            ogrenciTel:"",
            ogrenciMail:""
        }
    }),

    methods: {

        importExcel(e) {
          const files = e.target.files;
          if (!files.length) {
            return ;
          } else if (!/\.(xls|xlsx)$/.test(files[0].name.toLowerCase())) {
            return alert("Yükleme biçimi yanlış. Lütfen xls veya xlsx formatında dosya yükleyin");
          }
          const fileReader = new FileReader();
          fileReader.onload = ev => {
            try {
              const data = ev.target.result;
              const XLSX = xlsx;
              const workbook = XLSX.read(data, {
                type: "binary"
              });
              const wsname = workbook.SheetNames[0]; 
              const importedStudent = XLSX.utils.sheet_to_json(workbook.Sheets[wsname]);
              this.students = this.students.concat(importedStudent);
              this.addAutoSortNumbers();
              this.dialogImportExcel = false;
            } catch (e) {
              return alert("Read failure!"+e);
            }
          };
          fileReader.readAsBinaryString(files[0]);
          var input = document.getElementById("upload");
          input.value = "";
        },

        editItem(item) {
            this.editedIndex = this.students.indexOf(item);
            this.newStudent = Object.assign({}, item);
            this.dialogAddStudent = true;
        },

        closeDialog() {
            this.dialogAddStudent = false;
            this.dialogImportExcel = false;
            this.dialogComplete = false;
            this.dialogDelete = false;
        },

        save() {

            if (this.editedIndex > -1) {
               Object.assign(this.students[this.editedIndex], this.newStudent);        
            }
            else {
              this.newStudent.sıra =  (this.students.length == 0 ? 1 : this.students[this.students.length-1].sıra + 1);
              this.students.push(this.newStudent);
            }
            this.addAutoSortNumbers();
            this.closeDialog();
            this.dialogMessage = "Öğrenci başarıyla eklendi";
            this.dialogComplete = true;
            this.newStudent = [];
        },

        requiredControls(){
          
        },

        deleteStudents(){
          if(this.selected.length > 0){
            const tempList = this.students.filter(x => !this.selected.includes(x));
            this.students.splice(0,this.students.length)
            this.students = tempList;
          }
          this.dialogDelete = false;
          this.addAutoSortNumbers();
        },

        addAutoSortNumbers(){
          let number = 1;
          this.students.forEach( student => {
            student.sıra = number;
            number++;
          });
        },
    }
}
</script>

<style>
.required_field .v-label{
  color: #F44336;
  opacity: 1;
}
</style>