<template>
  <div class="flex">
    <v-data-table
      ref="table"
      v-model="selected"
      :single-select="singleSelect"
      item-key="ogrenciNo"
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
                @click="newStudent = []"
              >
                YENİ ÖĞRENCİ
              </v-btn>
            </template>

            <v-card>
              <v-card-title>
                <span class="text-h5 text mr-4">{{textHead}}</span>
                <sub class="text--secondary">{{textSub}}</sub>
              </v-card-title>

              <v-card-text>
                <v-container>
                  
                    <form 
                    id="app"
                    @submit="addStudent"
                    action="#"
                    >

                    <v-row>
                      
                      <v-col
                      cols="12"
                      sm="6"
                      md="4"
                      >
                      <input type="text" id="name" class="formItem" v-model="newStudent.ad" placeholder="*Adı" data-required-message="* Öğrenci Adını Giriniz" required>
                      </v-col>
                      <v-col
                      cols="12"
                      sm="6"
                      md="4"
                      >
                      <input type="text" id="lastname" class="formItem" v-model="newStudent.soyad" placeholder="*Soyadı" data-required-message="* Öğrenci Soyadını Giriniz" required>
                      </v-col>
                      <v-col
                      cols="12"
                      sm="6"
                      md="4"
                      >
                      <input type="text" id="studentNo" class="formItem" v-model="newStudent.ogrenciNo" placeholder="*Öğnreci No" data-required-message="* Öğrenci Numarasını Giriniz" required>
                      </v-col>
                      <v-col
                      cols="12"
                      sm="6"
                      md="4"
                      >
                      <input type="text" id="class" class="formItem" v-model="newStudent.sınıf" placeholder="*Sınıfı" data-required-message="* Öğrenci Sınıfını Giriniz" required>
                      </v-col>
                      <v-col
                      cols="12"
                      sm="6"
                      md="4"
                      >
                      <input type="text" id="classBranch" class="formItem" v-model="newStudent.sube" placeholder="*Şubesi" data-required-message="* Öğrenci Şubesini Giriniz" required>
                      </v-col>
                      <v-col
                      cols="12"
                      sm="6"
                      md="4"
                      >
                      <input type="text" id="gender" class="formItem" v-model="newStudent.cinsiyet" placeholder="Cinsiyeti (E/K)">
                      </v-col>
                      <v-col
                      cols="12"
                      sm="6"
                      md="4"
                      >
                      <input type="text" id="id" class="formItem" v-model="newStudent.tcno" placeholder="TC Kimlik No">
                      </v-col>
                      <v-col
                      cols="12"
                      sm="6"
                      md="4"
                      >
                      <input type="text" id="tel" class="formItem" v-model="newStudent.ogrenciTel" placeholder="Telefon No">
                      </v-col>
                      <v-col
                      cols="12"
                      sm="6"
                      md="4"
                      >
                      <input type="text" id="anneTel" class="formItem" v-model="newStudent.anneTel" placeholder="Anne Telefon No">
                      </v-col>
                      <v-col
                      cols="12"
                      sm="6"
                      md="4"
                      >
                      <input type="text" id="babaTel" class="formItem" v-model="newStudent.babaTel" placeholder="Baba Telefon No">
                      </v-col>
                      <v-col
                      cols="12"
                      sm="6"
                      md="4"
                      >
                      <input type="email" id="mail" class="formItem" v-model="newStudent.ogrenciMail" placeholder="Mail Adresi">
                      </v-col>
                      <v-col
                      cols="12"
                      sm="6"
                      md="4"
                      >
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
                          >
                            <input
                              type="submit"
                              value="EKLE"
                            >
                          </v-btn>
                            
                        </v-card-actions>
                      </v-col>
                    </v-row>
                    </form>
                </v-container>
              </v-card-text>

              
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

        <v-dialog v-model="dialogSave" max-width="1000px">        
            <template v-slot:activator="{ on, attrs }">
                <v-btn
                  class="ml-2 mb-2"
                  color="primary"
                  dark
                  v-bind="attrs"
                  v-on="on"
                >
                  KAYDET
                </v-btn>
            </template>

            <v-card>
              <v-card-title class="text-h5">Seçilen öğrencileri kaydetmek istediğinizden emin misiniz?</v-card-title>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" text @click="closeDialog">ÇIKIŞ</v-btn>
                <v-btn color="blue darken-1" text @click="saveStudents">KAYDET</v-btn>
                <v-spacer></v-spacer>
              </v-card-actions>
            </v-card>
        </v-dialog>

      </template>

    </v-data-table>

    <template>
      <div id="jsonField">

      </div>
    </template>
  </div>
</template>

<script>
import xlsx from "xlsx";

  export default {

    data: () => ({
        jsonData: JSON,
        dialogAddStudent: false,
        dialogImportExcel: false,
        dialogSave: false,
        dialogDelete: false,
        singleSelect: false,
        selected: [],
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
            { text: "Kayıt Durumu", value: "durumu" },
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
            ogrenciMail:"",
            durumu:""
        },
    }),
    watch: {
      students(){
        console.log("students değiştirildi")
      }
    },
    computed: {
      textHead () {
        return this.editedIndex === -1 ? 'YENİ ÖĞRENCİ' : 'ÖĞRENCİ BİLGİLERİNİ DÜZENLE'
      },
      textSub () {
        return this.editedIndex === -1 ? 'Öğrenci bilgilerini doldurunuz' : 'Öğrenci bilgilerini değiştirin'
      }
    },
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
              if(!this.checkStudentsNo(importedStudent)){
                alert("Aynı öğrenci nosuna sahip başka öğrenciler olduğu için bu liste eklenemez ! Lütfen kontrol ediniz.");
              }
              else{
                this.students = this.students.concat(importedStudent);
                this.students.forEach( student => {
                  student.durumu = "Kayıtlı Değil";
                });
                this.addAutoSortNumbers();
                this.dialogImportExcel = false;
              }
              
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
            this.dialogDelete = false;
            this.dialogSave = false;
            this.editedIndex = -1
        },

        addStudent() {
          this.newStudent.durumu = "Kayıtlı Değil";
          if(!this.checkStudentNo(this.newStudent)){
              alert("Aynı öğrenci numarasına sahip başka öğrenci olduğu için eklenemez !!");
          }
          else{
            if(this.editedIndex == -1){
              this.students.push(this.newStudent);
              this.closeDialog();
              this.newStudent = [];
              this.addAutoSortNumbers();
            }
            else{
              Object.assign(this.students[this.editedIndex], this.newStudent);    
              this.closeDialog();
              this.newStudent = [];
              this.editedIndex = -1;
            }             
          }
         
        },
        deleteStudents(){
          if(this.selected.length > 0){
            const tempList = this.students.filter(x => !this.selected.includes(x));
            this.students.splice(0,this.students.length)
            this.students = tempList;
          }
          this.selected = [];
          this.addJsonData();
          this.addAutoSortNumbers();
          this.dialogDelete = false;
        },

        addAutoSortNumbers(){
          let number = 1;
          this.students.forEach( student => {
            student.sıra = number;
            number++;
          });
        },

        checkStudentNo(checkedItem){
          if (this.editedIndex == -1){
            for ( let i = 0; i < this.students.length; i++) { 
            if(this.students[i].ogrenciNo == Number(checkedItem.ogrenciNo))
              return false;
            }
            return true;
          }
          else{
            for ( let i = 0; i < this.students.length; i++) {
              if( this.students[this.editedIndex].ogrenciNo == Number(checkedItem.ogrenciNo))
                return true; 
              else if(this.students[i].ogrenciNo == Number(checkedItem.ogrenciNo))
                return false;
            }
            return true;
          }
          

        },
        checkStudentsNo(checkedItems){
          for ( let i = 0; i < this.students.length; i++) { 
            for ( let j = 0; j < checkedItems.length; j++){
              if(this.students[i].ogrenciNo == Number(checkedItems[j].ogrenciNo))
                return false;
            }
          }
          return true;

        },
        addJsonData(){
          this.jsonData = JSON.stringify(this.students);
          document.getElementById('jsonField').innerHTML = this.jsonData;
        },

        saveStudents(){
          this.students = this.students.map((x)=>{
            x.durumu = "kayıtlı";
            return x;
          })
          
          this.dialogSave = false; 
          return this.addJsonData();
          
      },
        
        
    }
}
</script>

<style>
.required_field .v-label{
  color: #F44336;
  opacity: 1;
}
.formItem{
  width: 100%;
  padding: 12px 20px;
  margin: 8px 0;
  display: inline-block;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
}

input:required::placeholder {
    font-weight: bold;
    opacity: .5;
    color: red;
}

</style>