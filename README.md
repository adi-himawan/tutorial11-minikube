## Tutorial 11 - Minikube

#### Reflection on Hello Minikube
#### 1. Compare the application logs before and after you exposed it as a Service. Try to open the app several times while the proxy into the Service is running. What do you see in the logs? Does the number of logs increase each time you open the app?
Sebelum Pod di-expose sebagai Service
![Sebelum Pod di-expose sebagai Service](/img/commit1_1.png)

Sesudah Pod di-expose sebagai Service
![Sesudah Pod di-expose sebagai Service](/img/commit1_2.png)

Terdapat perbedaan pada logs aplikasi sebelum dan sesudah Pod di-expose sebagai Service. Ketika Pod sudah di-expose sebagai Service, logs menampilkan setiap request yang masuk ke Service. Seperti yang terlihat pada lampiran di atas, ada beberapa entri berupa GET request pada endpoint "/" yang ditampilkan pada logs setelah Pod di-expose sebagai service. Logs ini merupakan hasil dari mengakses Service hello-node secara berulang kali. Alhasil, setiap kali aplikasi di-refresh atau dibuka pada browser, jumlah entri di logs akan terus bertambah. Sebagai kesimpulan, hasil logs menunjukkan aplikasi akan terus menangani setiap request yang masuk setelah Pod di-expose sebagai Service.

#### 2. Notice that there are two versions of `kubectl get` invocation during this tutorial section. The first does not have any option, while the latter has `-n` option with value set to `kube-system`. What is the purpose of the `-n` option and why did the output not list the pods/services that you explicitly created?
Opsi `-n` digunakan untuk menentukan namespace saat menjalankan command kubectl. Secara sederhana, namespace dapat didefinisikan sebagai sebuah grup yang dibuat oleh Kubernetes untuk mengisolasi resources dalam satu cluster menjadi beberapa grup. Oleh karena itu, ketika kita menjalankan command `kubectl get pods,services -n kube-system`, sistem hanya akan menampilkan pods dan services yang sudah didefinisikan dalam namespace `kube-system`, sebuah grup untuk objek-objek yang dibuat oleh sistem Kubernetes. Akibatnya, sistem tidak akan menampilkan pods dan services yang telah didefinisikan oleh pengguna secara eksplisit.

#### Reflection on Rolling Update & Kubernetes Manifest File
#### 1. What is the difference between Rolling Update and Recreate deployment strategy?
Perbedaan utama antara strategi Rolling Update dan Recreate terletak pada cara keduanya melakukan transisi dari versi aplikasi lama ke versi aplikasi baru. Sistem dengan strategi Rolling Update akan melakukan transisi secara perlahan, di mana pods baru akan secara bertahap mengambil alih peran dari pods yang lama. Sementara itu, sistem dengan strategi Recreate akan menonaktifkan pods lama secara bersamaan sebelum mulai meluncurkan pods baru. Dibanding strategi Rolling Update, strategi Recreate dapat menyebabkan downtime singkat selama proses updating berlangsung.

#### 2. Try deploying the Spring Petclinic REST using Recreate deployment strategy and document your attempt.
Melakukan set up springboot-petclinic-rest versi 3.0.2.
![](/img/commit2_1.png)
![](/img/commit2_2.png)

Ketika kita men-delete pods, sistem langsung menonaktifkan pods lama dan meluncurkan pods baru. Hal ini sesuai dengan strategi Recreate.
![](/img/commit2_3.png)

Setelah pods kembali running, service dapat berjalan seperti biasa.
![](/img/commit2_4.png)
![](/img/commit2_5.png)

#### 3. Prepare different manifest files for executing Recreate deployment strategy.
Telah dibuat sebuah manifest file bernama newdeployment.yaml. File ini memiliki isi yang sangat mirip dengan deployment.yaml. Satu-satunya perbedaan antara kedua manifest file tersebut adalah bagian berikut.
```
  selector:
    matchLabels:
      app: spring-petclinic-rest
  strategy:
    type: Recreate
```
Nantinya, file ini dapat digunakan seperti biasa dengan command `kubectl apply -f`.

#### 4. What do you think are the benefits of using Kubernetes manifest files? Recall your experience in deploying the app manually and compare it to your experience when deploying the same app by applying the manifest files (i.e., invoking `kubectl apply -f` command) to the cluster.
Menurut saya, salah satu kelebihan menggunakan manifest file adalah membuat kita tidak perlu menjalankan proses deployment dari awal lagi. Berdasarkan pengalaman saya, menjalankan proses deployment tanpa menggunakan manifest file relatif lebih sulit karena terdapat begitu banyak langkah yang berpotensi menimbulkan error. Sebaliknya, melakukan proses deployment dengan menggunakan manifest file terasa jauh lebih mudah karena kita cukup menuliskan command `kubectl apply -f`. 