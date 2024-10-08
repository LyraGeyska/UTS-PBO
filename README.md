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

### 7. Menampilkan Data Utama 
Pada file java, buat fungsi method tampil untuk menampilkan data dalam tabel pada saat aplikasi dijalankan pertama kali :
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






