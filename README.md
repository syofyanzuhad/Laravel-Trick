# Laravel Trik
Kumpulan trik berbahasa indonesia untuk menggunakan framework laravel.

_Berisi: **7** trik._

**Terakhir diupdate 17 Oktober 2020**

> Berikan pull request untuk memberikan manfaat lebih banyak !

# Daftar Isi :

- [DB Models dan Eloquent](#db-models-dan-eloquent) (2 trik).
- [Perintah `artisan`](#perintah-artisan) (1 trik).
- [Package](#package) (2 trik).
- [Lain - lain](#lain-lain) (1 trik).

## DB Models dan Eloquent

⬆️ [Ke Atas](#laravel-trik) ➡️ [Berikutnya (Perintah `artisan`)](#perintah-artisan)

- [Cara mengubah format output `created_at` dan `updated_at` lewat model](#cara-mengubah-format-output-created_at-dan-updated_at-lewat-model)

- [Cara Mengubah format text `created_at` dan `updated_at` menjadi `tgl_dibuat` dan `tgl_diupdate` lewat model](#cara-mengubah-format-text-created_at-dan-updated_at-menjadi-tgl_dibuat-dan-tgl_diupdate-lewat-model)

### **Cara mengubah format output `created_at` dan `updated_at` lewat model**

- **created_at**

Untuk mengubah format `created_at`, tambahkan method berikut di dalam model:

```
public function getCreatedAtFormattedAttribute()
{
   return $this->created_at->format('H:i d, M Y');
}
```
Method ini bisa digunakan dengan cara seperti ini : `$entry->created_at_formatted`.

Dan akan menampilkan hasil seperti ini: `04:19 23, Aug 2020`.

- **updated_at**

Untuk mengubah format `updated_at`, tambahkan method berikut di dalam model:

```
public function getUpdatedAtFormattedAttribute()
{
   return $this->updated_at->format('H:i d, M Y');
}
```
Method ini bisa digunakan dengan cara seperti ini : `$entry->updated_at_formatted`.

Dan akan menampilkan hasil seperti ini: `04:19 23, Aug 2020`.

### **Cara Mengubah format text created_at dan updated_at menjadi tgl_dibuat dan tgl_diupdate lewat model**

Caranya sangat mudah 

- **created_at**

Untuk created_at cukup menambahkan :

     const CREATED_AT = 'tgl_dibuat';

- **updated_at**

dan untuk  updated_at cukup menambahkan:

    const UPDATED_AT = 'tgl_diupdate';

## Perintah `artisan`

⬆️ [Ke Atas](#laravel-trik) ➡️ [Berikutnya (Package)](#package)

- [Cara membuat model, controller, migration sekaligus](#cara-membuat-model-controller-migration-sekaligus)

### **Cara membuat model, controller, migration sekaligus**

Untuk membuat `model`, `controller`, dan `migration` sekaligus cukup jalankan perintah untuk membuat model dengan tambahan `-mc`, seperti berikut :

```
php artisan make:model User -mc
```

Jika kita ingin membuat `controller` dengan resource (default method dari laravel). Gunakan perintah ini (dengan tambahan huruf `r`):
```
php artisan make:model User -mcr
```
## Package

⬆️ [Ke Atas](#laravel-trik) ➡️ [Berikutnya (Helpers)](#Helpers)

- [Install Spatie Role Permission](#install-spatie-role-permission)
- [Cara Menggunakan Role](#cara-menggunakan-role)

### **Install Spatie Role Permission**

1. Install package

 `composer require spatie/laravel-permission`
 
2. edit app/config.php
```
'providers' => [
    // ...
    Spatie\Permission\PermissionServiceProvider::class,
];
```
3. publish migration di terminal

`php artisan vendor:publish --provider="Spatie\Permission\PermissionServiceProvider"`

4. clear config & cache

 `php artisan optimize:clear`
 **or**
 `php artisan config:clear`
 
5. terakhir jalankan migration

 `php artisan migrate`

### Cara menggunakan Role
1. membuat role 
```
use Spatie\Permission\Models\Role;

$role = Role::create(['name' => 'writer']);
```

2. Menambahkan role kepada user
```
$user->assignRole($role);
```
3. Menghapus role pada user
```
$user->revokeRoleTo($role); //$role = nama rolenya apa
```

## Helpers
⬆️ [Ke Atas](#laravel-trik) ➡️ [Berikutnya (Lain -lain)](#lain-lain)

### **Membuat Tanggal Indonesia**

1. Membuat folder bernama Helper didalam Folder App
2. Membuat file helper dengan Nama TanggalIndonesia.php
3. Membuat 1 function dengan parameter $tgl, $tampilHari = true
```

	public function tanggalIndonesia($tgl, $tampilHari = true) 
	{

	}

```
4. Membuat Codingan didalam function
```
	
	$nama_hari = array("Ahad", "Senin", "Selasa", "Rabu", "Kamis", "Jum'at", "Sabtu");
        $nama_bulan = array(1=>"Januari", "Februari", "Maret", "April", "Mei", "Juni", "Juli", "Agustus", "September", "Oktober", "November", "Desember");
        
        $tahun = substr($tgl,0,4);
        $bulan = $nama_bulan[(int)substr($tgl,5,2)];
        $tanggal = substr($tgl,8,2);

        $text = "";

        if ($tampilhari) {
            $urutan_hari = date('w', mktime(0,0,0, substr($tgl,5,2), $tanggal, $tahun));
            $hari = $nama_hari[$urutan_hari];
            $text .= $hari.", ";
        }

        $text .= $tanggal ." ". $bulan ." ". $tahun;
        
        return $text;
```

### **Pengunaan Helper Tanggal Indonesia**
```
tanggalIndonesia(date('Y-m-d',strtotime($user->created_at)));

```
	

## Lain-lain
⬆️ [Ke Atas](#laravel-trik)

- [Penulisan singkat `if-else` dan `if-elseif-else` dengan ternary](#penulisan-singkat-if-else-dan-if-elseif-else-dengan-ternary)

### **Penulisan singkat if-else dan if-elseif-else dengan ternary**

- **if-else**

Penulisan singkat if-else yaitu: 

    $role = 0;
    echo $role == 0 ? 'Admin' : 'Pengunjung';

- **if-elseif-else**

**Hindari** penulisan seperti ini: 

> $role = 0;

> echo $role == 0 ? 'admin' : $role == 1 ? 'guru' : 'santri';
	
Jika mengikuti penulisan di atas maka akan muncul pesan error
**`Unparenthesized a ? b : c ? d : e is deprecated. Use either (a ? b : c) ? d : e or a ? b : (c ? d : e)`**

Yang benar adalah seperti ini:
    
    $role = 0;
    echo $role == 0 ? 'admin' : ($role == 1 ? 'guru' : 'santri'); 


