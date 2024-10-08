# Aplikasi CRUD Data Matakuliah Dengan Menggunakan Jawa Swing dan Database PostgreSQL
Ujian Tengah Semester 3 Pemrograman Berorientasi Obyek

## Deskripsi Penugasan
Pada Projek Simulasi UTS Pemrograman Berbasis Objek (PBO) ini, diminta untuk menyelesaikan soal soal yaitu mengimplementasikan penggunaan CRUD pada Netbeans menggunakan bahasa pemrograman Java dan database PostgreSQL. Penggunaan CRUD kali ini akan dibangun dengan bantuan Java Swing untuk mendukung operasi CRUD tersebut.

Langkah-langkah Membuat CRUD dengan Java :
### 1. Membuat database baru pada PostgreSQL.
### 2. Membuat tabel baru dengan atribut yang telah ditentukan. Dalam projek ini memiliki atribut tabel berikut : KodeMK, SKS, NamaMK, SemesterAjar.
### 3. Membuat Project baru pada netbeans.
### 4. Membuat file JFrame Form pada Project Package yang telah dibuat.
### 5. Mengkoneksikan PostgreSQL dengan java menggunakan JDBC.
Berikut adalah code java untuk menghubungkan dabatabe PostgreSQL menggunakan JDBC :
<pre>
  Connection conn;
    Statement stmt;
    PreparedStatement pstmt;

    String driver = "org.postgresql.Driver";
    String koneksi = "jdbc:postgresql://localhost:5432/nama_database";
    String user = "postgres";
    String password = "lyra123";

    InputStreamReader inputStreamReader = new InputStreamReader(System.in);
    BufferedReader input = new BufferedReader(inputStreamReader);
</pre>

### 6. Membuat User Inferface GUI menggunakan Java Swing
Buat tampilan aplikasi menggunakan Java Swing dengan beberapa fitur yang tersedia dan yang diperlukan,
- **Label**, untuk membuat text baik judul maupun keterangan lainnya,
 (DATA MATAKULIAH, KODE MK, SKS, MATAKULIAH, SEMESTER AJAR).
![image](https://github.com/user-attachments/assets/e14945ec-0d84-43e6-a682-ecbaadcb4e66)

- **Text Field**, untuk membuat wadah/tempat proses penginputan data data,
  (txtKodeMK, txtSks, txtMatakuliah, txtSemester).
  ![image](https://github.com/user-attachments/assets/fd8bc279-c039-4289-bb26-6e0becf235aa)

- **Button**, untuk membuat tombol pengoprasian pada aplikasi nantinya,
  (btnBaru, btnSimpan, btnupdate, btnDelete, btnExit).
  ![image](https://github.com/user-attachments/assets/87d0018e-cb8f-44e8-8760-6d18a9810036)

- **Table**, untuk membuat tabel pada aplikasi yang nantinya akan digunakan untuk menampilkan hasil dari transaksi.
![image](https://github.com/user-attachments/assets/fc412a95-67d8-46c9-8e30-a26c7203b9d7)

- **Hasilnya :**
![image](https://github.com/user-attachments/assets/f12b18c1-8ae5-4e71-a0c1-c68f267f942b)

### 7. Menampilkan Data Utama Pada file java, buat fungsi method tampil untuk menampilkan data dalam tabel pada saat aplikasi dijalankan pertama kali :
<pre>
  public void tampil() {
        try {
            // TODO code application logi
            Class.forName(driver);
            String sql = "SELECT * FROM matakuliah";
            conn = DriverManager.getConnection(koneksi, user, password);
            stmt = conn.createStatement();

            while (!conn.isClosed()) {

                ResultSet rs = stmt.executeQuery(sql);
                this.tblHasil.setModel(pertemuankelima.DbUtils.resultSetToTableModel(rs));
                while (rs.next()) {
                    System.out.println(String.valueOf(rs.getObject(1)) + " "
                            + String.valueOf(rs.getObject(2)) + " "
                            + String.valueOf(rs.getObject(3)) + " " + String.valueOf(rs.getObject(4)));
                }
                conn.close();
            }

            stmt.close();

        } catch (ClassNotFoundException ex) {
            Logger.getLogger(BayuFrame.class.getName()).log(Level.SEVERE, null, ex);
        } catch (SQLException ex) {
            Logger.getLogger(BayuFrame.class.getName()).log(Level.SEVERE, null, ex);
        }
    }
</pre>

- Hasil dari code diatas :
  ![image](https://github.com/user-attachments/assets/02ba04eb-0cd9-4415-bfa8-21715a6b8e9f)



### 8. Penggunaan btnBaru
**Button Baru** ini digunakan untuk membuat atau memasukkan data baru pada text field, tombol ini akan berfungsi hanya jika pengguna menekan tombol ini untuk memasukkan data yang baru.
- Tambahkan code berikut, untuk mengaktifkan penggunaan **btnBaru**
  <pre>
    private void btnBaruActionPerformed(java.awt.event.ActionEvent evt) {                                        
        // TODO add your handling code here:
        txtKodeMK.setText("");
        txtSks.setText("");
        txtMatakuliah.setText("");
        txtSemester.setText("");
    }     
  </pre>

### 9. Pembuatan dan Penggunaan btnSimpan
**Button Simpan** ini nantinya akan digunakan untuk transaksi menyimpan data yang baru dimasukkan atau diinputkan.
- Tambahkan code berikut untuk membuat fungsi pada Button Simpan :
  <pre>
     private void btnSimpanActionPerformed(java.awt.event.ActionEvent evt) {                                          
        // TODO add your handling code here:
        if (txtKodeMK.getText().equals("")) {
            JOptionPane.showMessageDialog(null, "Isi semua data");
        } else if (txtSks.getText().equals("")) {
            JOptionPane.showMessageDialog(null, "Isi semua data");
        } else if (txtMatakuliah.getText().equals("")) {
            JOptionPane.showMessageDialog(null, "Isi semua data");
        } else if (txtSemester.getText().equals("")) {
            JOptionPane.showMessageDialog(null, "Isi semua data");
        } else {
            try {
                Class.forName(driver);
                conn = DriverManager.getConnection(koneksi, user, password);
                conn.setAutoCommit(false);

                String sql = "INSERT INTO matakuliah VALUES(?,?,?,?)";
                pstmt = conn.prepareStatement(sql);

                String kodeMK, sks, namaMK, semesterAjar;
                kodeMK = txtKodeMK.getText();
                sks = txtSks.getText();
                namaMK = txtMatakuliah.getText();
                semesterAjar = txtSemester.getText();

                pstmt.setString(1, kodeMK);
                pstmt.setLong(2, Long.parseLong(sks));
                pstmt.setString(3, namaMK);
                pstmt.setLong(4, Long.parseLong(semesterAjar));

                pstmt.executeUpdate();
                conn.commit();
                pstmt.close();
                conn.close();
                bersih();

                JOptionPane.showMessageDialog(null, "Data Berhasil Ditambahkan");
            } catch (ClassNotFoundException | SQLException ex) {
                JOptionPane.showMessageDialog(null, "Terjadi Kesalahan Saat Pengisian Data");
            }
        }
        tampil();
    }                 
  </pre>
  Pada code diatas yang paling akhir, ditambahkan method untuk memanggil method tampil agar data yang berhasil tersimpan akan otomatis ditambahkan pada tabel tampilan.
  <pre>
    tampil();
  </pre>

  - Berikut adalah tampilan dan penggunaan Button Simpan :
    ![image](https://github.com/user-attachments/assets/1aee432f-025c-4b7a-9bf8-fbd4f6348873)
    Data yang berhasil disimpan akan muncul pemberitahuan.
    
### 10. Pembuatan dan Penggunaan btnUpdate
- Sebelumnya, aktifkan Key Table Mouse Clicked untuk menampilkan data yang dipilih dari dalam tabel.
- Masukkan code berikut kedalam Button Update :
  <pre>
    private void btnUpdateActionPerformed(java.awt.event.ActionEvent evt) {                                          
        // TODO add your handling code here:
        String kodeMK, sks, namaMK, semesterAjar;
        if (txtKodeMK.getText().equals("")) {
            JOptionPane.showMessageDialog(null, "Isi semua data");
        } else if (txtSks.getText().equals("")) {
            JOptionPane.showMessageDialog(null, "Isi semua data");
        } else if (txtMatakuliah.getText().equals("")) {
            JOptionPane.showMessageDialog(null, "Isi semua data");
        } else if (txtSemester.getText().equals("")) {
            JOptionPane.showMessageDialog(null, "Isi semua data");
        } else {
            try {
                Class.forName(driver);
                String sql = "UPDATE matakuliah SET sks = ?, namaMK = ?, semesterAjar = ? WHERE kodeMK = ?";
                conn = DriverManager.getConnection(koneksi, user, password);
                pstmt = conn.prepareStatement(sql);

                kodeMK = txtKodeMK.getText();
                sks = txtSks.getText();
                namaMK = txtMatakuliah.getText();
                semesterAjar = txtSemester.getText();

                pstmt.setLong(1, Long.parseLong(sks));
                pstmt.setString(2, namaMK);
                pstmt.setLong(3, Long.parseLong(semesterAjar));
                pstmt.setString(4, kodeMK);

                int rowsAffected = pstmt.executeUpdate();
                if (rowsAffected > 0) {
                    JOptionPane.showMessageDialog(null, "Data Berhasil Diupdate");
                    pstmt.close();
                    conn.close();
                    bersih();
                } else {
                    JOptionPane.showMessageDialog(null, "Data Tidak Ditemukan");
                }
            } catch (ClassNotFoundException | SQLException ex) {
                Logger.getLogger(entitasMatakuliah.class.getName()).log(Level.SEVERE, null, ex);
            }
        }
        tampil();
    }               
  </pre>
- Coba jalankan lagi aplikasi tersebut dan lakukan update data
  ![image](https://github.com/user-attachments/assets/72e2e595-c923-4119-8e20-4c79f2412ad4)

### 11. Pembuatan dan Penggunaan btnDelete
- Pada **Button Delete** ini, akan menggunakan Events MouseClicked pada tabel untuk mengakses data agar dapat diedit.
- Klik 2 kali pada design GUI **BUTTON DELETE**, otomatis akan diarahkan pada lokasi code tsb.
- Masukkan code dibawag ini :
  <pre>
   private void btnDeleteActionPerformed(java.awt.event.ActionEvent evt) {                                          
        // TODO add your handling code here:
        String kodeMK;
        kodeMK = txtKodeMK.getText();

        try {
            Class.forName(driver);
            conn = DriverManager.getConnection(koneksi, user, password);

            int jawab = JOptionPane.showConfirmDialog(null, "Apakah Anda Yakin Akan Menghapus Data Ini?");
            switch (jawab) {
                case JOptionPane.YES_OPTION:
                    String deleteSql = "DELETE FROM matakuliah WHERE kodeMK = ?";
                    pstmt = conn.prepareStatement(deleteSql);
                    pstmt.setString(1, kodeMK);
                    pstmt.executeUpdate();
                    pstmt.close();
                    conn.close();
                    bersih();
                    break;
                case JOptionPane.NO_OPTION:
                    JOptionPane.showMessageDialog(this, "Data Tidak Jadi Dihapus");
                    break;
            }
        } catch (ClassNotFoundException | SQLException ex) {
            JOptionPane.showMessageDialog(null, "Periksa Kembali");
        }
        tampil();
    }            
  </pre>
- Berikut adalah tampilan program apabila dijalankan :
  ![image](https://github.com/user-attachments/assets/ad8cd09c-bde8-4589-9a3f-b8766ca166bd)





  
  








