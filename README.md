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