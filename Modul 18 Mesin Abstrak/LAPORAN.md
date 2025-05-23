<h1 align="center">Laporan Praktikum Modul 18 <br>Mesin Abstrak</h1>
<p align="center">SYAHLA KHEISYA MAYASTRIA - 103112430018</p>

## Dasar Teori
Mesin abstrak adalah model komputasi yang dirancang di atas model mesin komputasi yang telah ada, yaitu tipe data dan operasi-operasi dasarnya dibuat menggunakan tipe data dan operasi-operasi yang tersedia di mesin di bawahnya. Teknik ini merupakan salah satu cara untuk membangun perangkat lunak.
### Soal Latihan Modul 18 Mesin Abstrak

#### Soal 1

>Implementasi mesin abstrak karakter yang bekerja terhadap untaian karakter (yang diakhiri dengan penanda titik (".") dan mempunyai sejumlah operasi dasar. 
>a) Operasi dasar mesin karakter: 
>➢ Prosedur start(); yang menyiapkan mesin karakter di awal rangkaian karakter. 
>➢ Prosedur maju(); yang memajukan pembaca ke posisi karakter berikutnya. 
>➢ Fungsi eop(); yang mengembalikan nilai true apabila sudah mencapai akhir rangkaian, sampai ke penanda titik ("."). 
>➢ Fungsi cc(); yang mengembalikan karakter yang sedang terbaca, atau berada pada posisi pembacaan mesin. 
>b) Dengan operasi dasar di atas buat algoritma untuk: 
>➢ Membaca seluruh karakter yang diberikan ke mesin karakter tersebut. 
>➢ Menghitung berapa banyak karakter yang terbaca. 
>➢ Menghitung ada berapa huruf "A" yang terbaca. 
>➢ Menghitung frekuensi kemunculan huruf "A" terhadap seluruh karakter terbaca. 
>➢ Menghitung ada berapa kata "LE" (pasangan berturutan huruf "L" dan "E") yang terbaca.

```go
package main

import (
    "bufio"
    "fmt"
    "os"
    "strings"
)

type MesinKarakter struct {
    input   string
    posisi  int
    panjang int
}

func (m *MesinKarakter) start() {
    m.posisi = 0
}

func (m *MesinKarakter) maju() {
    if m.posisi < m.panjang {
        m.posisi++
    }
}

func (m *MesinKarakter) eop() bool {
    if m.posisi >= m.panjang {
        return true
    }
    return m.input[m.posisi] == '.'
}

func (m *MesinKarakter) cc() byte {
    if m.posisi < m.panjang {
        return m.input[m.posisi]
    }
    return 0
}

func main() {
    reader := bufio.NewReader(os.Stdin)
    fmt.Print("Masukkan kalimat (akhiri dengan titik '.'): ")
    inputText, _ := reader.ReadString('\n')
    inputText = strings.TrimSpace(inputText)
    inputText = strings.ToUpper(inputText)

    mesin := MesinKarakter{
        input:   inputText,
        panjang: len(inputText),
    }
  
    mesin.start()

    var totalKarakter, jumlahA, jumlahLE int
    var prevChar byte = 0
    
    for !mesin.eop() {
        ch := mesin.cc()
        totalKarakter++

        if ch == 'A' {
            jumlahA++
        }

        if prevChar == 'L' && ch == 'E' {
            jumlahLE++
        }

        prevChar = ch
        mesin.maju()
    }

    frekuensiA := 0.0
    if totalKarakter > 0 {
        frekuensiA = float64(jumlahA) / float64(totalKarakter)
    }

    fmt.Printf("\nJumlah karakter terbaca: %d\n", totalKarakter)
    fmt.Printf("Jumlah huruf 'A' terbaca: %d\n", jumlahA)
    fmt.Printf("Frekuensi huruf 'A': %.4f\n", frekuensiA)
    fmt.Printf("Jumlah pasangan 'LE' terbaca: %d\n", jumlahLE)
}
```
![Output](Modul%2018%20Mesin%20Abstrak/output/soal4.png)

Program ini diguankan untuk membaca sebuah kalimat dari pengguna, kemudian menganalisis isi dari kalimat tersebut. Kalimat harus diakhiri dengan tanda titik (`.`), karena titik itu menjadi penanda bahwa program sudah boleh berhenti membaca.
Begitu program dijalankan, pengguna akan diminta untuk memasukkan kalimat. Setelah kalimat diketik dan dikirim, program akan mulai membacanya satu per satu, karakter demi karakter, dari awal hingga titik.
Sebelum diproses, kalimat yang dimasukkan akan diubah menjadi huruf besar semua. Ini penting supaya program tidak membedakan antara huruf kecil dan huruf besar. Jadi, 'a' dan 'A' akan dianggap sama, begitu juga 'le' dan 'LE'.