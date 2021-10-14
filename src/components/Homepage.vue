<template>
 <div class="container">
  <div class="text-center">
    <h1>Input Data Warga</h1>
    <h6>Aplikasi Input Data Warga untuk Bantuan di Era Pandemi Covid-19</h6>
  </div>
  <hr>
  <form @submit.prevent="inputData()" ref="form"> 
      <div class="row">
          <div class="col-lg-6">
              <div class="form-group mb-3 mb-3">
                    <label>Nama :</label>
                    <input type="text" class="form-control" v-model="name"> 
                </div>
                <div class="form-group mb-3">
                    <label>Nomor Induk Kepegawaian (NIK) :</label>
                    <input type="text" class="form-control" v-model="nik">
                </div>
                <div class="form-group mb-3">
                    <label>Nomor Kartu Keluarga :</label>
                    <input type="text" class="form-control" v-model="no_kk">
                </div>
                <div class="form-group mb-3">
                    <label>Foto KTP <small>(Max 2MB, Format JPG/JPEG/PNG/BMP)</small>:</label>
                    <input type="file" class="form-control" id="foto_ktp" ref="foto_ktp" v-on:change="cekFotoKTP()">
                    <span class="text-danger">{{error1}}</span>
                </div>
                <div class="form-group mb-3">
                    <label>Foto Kartu Keluarga <small>(Max 2MB, Format JPG/JPEG/PNG/BMP)</small> :</label>
                    <input type="file" class="form-control" id="foto_kartu_keluarga" ref="foto_kartu_keluarga" v-on:change="cekFotoKK()">
                    <span class="text-danger">{{error2}}</span>
                </div>
                <div class="form-group mb-3">
                    <label>Umur :</label>
                    <input type="number" class="form-control" v-model="umur" >
                </div>
                <div class="form-group mb-3">
                    <label>Jenis Kelamin :</label>
                    <div class="custom-control custom-radio custom-control-inline">
                        <input type="radio" name="jk" class="custom-control-input" style="margin-right:5px" value="1" v-model="jk">
                        <label class="custom-control-label" >Laki-Laki</label>
                    </div>
                    <div class="custom-control custom-radio custom-control-inline">
                        <input type="radio" name="jk" class="custom-control-input" style="margin-right:5px" value="0" v-model="jk">
                        <label class="custom-control-label" >Perempuan</label>
                    </div>
                </div>
          </div>
          <div class="col-lg-6">
            <div class="form-group mb-3">
                <label>Alamat :</label>
                <textarea class="form-control" rows="3" style="resize:none;" v-model="alamat"></textarea>
            </div>
            <div class="form-group mb-3">
                <label>RT :</label>
                <input type="number" class="form-control" min="0" v-model="rt">
            </div>
            <div class="form-group mb-3">
                <label>RW :</label>
                <input type="number" class="form-control" min="0" v-model="rw">
            </div>
            <div class="form-group mb-3">
                <label>Penghasilan Sebelum Pandemi :</label>
                <div class="input-group mb-3">
                    <div class="input-group-prepend">
                        <span class="input-group-text" id="basic-addon1">Rp</span>
                    </div>
                    <input type="number" class="form-control" min="0" v-model="penghasilan_sebelum">
                </div>
            </div>
            <div class="form-group mb-3">
                <label>Penghasilan Setelah Pandemi :</label>
                <div class="input-group mb-3">
                    <div class="input-group-prepend">
                        <span class="input-group-text" id="basic-addon1">Rp</span>
                    </div>
                    <input type="number" class="form-control" v-model="penghasilan_sesudah">
                </div>
            </div>
            <div class="form-group mb-3">
                <label>Alasan Membutuhkan Bantuan :</label>
                <select class="form-control" @change="cekAlasan()" v-model="alasan">
                    <option value="" disabled selected>Pilih alasan</option>
                    <option value="1">Kehilangan Pekerjaan</option>
                    <option value="2">Kepala keluarga terdampak atau korban Covid</option>
                    <option value="3">Tergolong fakir/miskin semenjak sebelum Covid</option>
                    <option value="4">Lainnya : ....</option>
                </select>
                <input type="text" class="form-control mt-2 hide" id="alasanLainnya" v-model="txtAlasan">
            </div>
          </div>
        </div>
        <hr>
            <div class="form-group">
                <input type="checkbox" id="term" class="pr-5" style="margin-right:5px" v-model="term" />
                <label>Saya menyatakan bahwa data yang diisikan adalah benar dan siap mempertanggungjawabkan apabila ditemukan ketidaksesuaian dalam data tersebut.</label>
            </div>
        <div class="text-center">
            <button type="submit" class="btn btn-primary mt-2">Kirim</button>
        </div>
        <div class="alert alert-danger mt-3 hide" id="alert" role="alert">
            Silahkan centang kolom pernyataan
        </div>
  </form>
 </div> 
</template>
<script>
export default {
  data: () => ({
    name : '',
    nik : '',
    no_kk : '',
    foto_ktp : '',
    foto_kartu_keluarga : '',
    umur : '',
    jk : '',
    alamat : '',
    txtAlasan:'',
    rt : '',
    rw : '',
    penghasilan_sebelum : '',
    penghasilan_sesudah : '',
    alasan : '',
    error1 :'',
    error2 :'',
    term : '',
    alert : 'show'
  }),
  methods : {
    cekFotoKTP(){
        this.foto_ktp = this.$refs.foto_ktp.files[0];
        let fileType=this.foto_ktp.type
        let fileSize=this.foto_ktp.size
        let fileAccept = ['image/jpeg', 'image/bmp', 'image/png']
        if (fileSize<=2097152) {
            if (!fileAccept.includes(fileType)) {
                this.error1='File KTP Tidak Sesuai'                
                this.foto_ktp=''
                document.getElementById('foto_ktp').value=''
            }else{
                this.error1=''
            }
        }else{
            this.error1='File KTP Tidak Sesuai'
            this.foto_ktp=''
            document.getElementById('foto_ktp').value=''
        }
    },
    cekFotoKK(){
        this.foto_kartu_keluarga = this.$refs.foto_kartu_keluarga.files[0];
        let fileType=this.foto_kartu_keluarga.type
        let fileSize=this.foto_kartu_keluarga.size
        let fileAccept = ['image/jpeg', 'image/bmp', 'image/png']
        if (fileSize<=2097152) {
            if (!fileAccept.includes(fileType)) {
                this.error2='File KK Tidak Sesuai'
                this.foto_kartu_keluarga=''
                document.getElementById('foto_kartu_keluarga').value=''
            }else{
                this.error2=''
            }
        }else{
            this.error2='File KK Tidak Sesuai'
            this.foto_kartu_keluarga=''
            document.getElementById('foto_kartu_keluarga').value=''
        }
    },
    cekAlasan(){
        switch (this.alasan) {
            case '1':
                this.txtAlasan="Kehilangan Pekerjaan"
                document.getElementById('alasanLainnya').classList.remove("unhide");
                document.getElementById('alasanLainnya').classList.add("hide");
                break;
            case '2':
                this.txtAlasan="Kepala keluarga terdampak atau korban Covid"
                document.getElementById('alasanLainnya').classList.remove("unhide");
                document.getElementById('alasanLainnya').classList.add("hide");
                break;
            case '3':
                this.txtAlasan="Tergolong fakir/miskin semenjak sebelum Covid"
                document.getElementById('alasanLainnya').classList.remove("unhide");
                document.getElementById('alasanLainnya').classList.add("hide");
                break;
            default:
                document.getElementById('alasanLainnya').classList.remove("hide");
                document.getElementById('alasanLainnya').classList.add("unhide");
                document.getElementById('alasanLainnya').focus();
                this.txtAlasan=''
                break;
        }
    },
    inputData(){
        if (!this.term) {
            document.getElementById('alert').classList.remove("hide");
            document.getElementById('alert').classList.add("unhide");
        }else{
            let dataWarga = new FormData();
            dataWarga.append('name',this.name)
            dataWarga.append('nik',this.nik)
            dataWarga.append('no_kk',this.no_kk)
            dataWarga.append('foto_ktp',this.foto_ktp)
            dataWarga.append('foto_kartu_keluarga',this.foto_kartu_keluarga)
            dataWarga.append('umur',this.umur)
            dataWarga.append('jk',this.jk)
            dataWarga.append('alamat',this.alamat)
            dataWarga.append('rt',this.rt)
            dataWarga.append('rw',this.rw)
            dataWarga.append('penghasilan_sebelum',this.penghasilan_sebelum)
            dataWarga.append('penghasilan_sesudah',this.penghasilan_sesudah)
            dataWarga.append('alasan',this.txtAlasan)

            console.log(dataWarga)
            for (var value of dataWarga.values()) {
                console.log(value);
            }
            
            document.getElementById('alert').classList.remove("unhide");
            document.getElementById('alert').classList.add("hide");
        }
    }
  }
}
</script>
