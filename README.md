# Cara untuk Deployment dan Autoscaling menggunakan Kubernetes

**File ini menjelaskan cara menggunakan manifest Kubernetes yang disediakan untuk mendeploy dan meng-autoscaling aplikasi DevOps bernama "devops".**

### Deployment

* **Nama:** `devops`
* **Image:** `rain5115/devops:1.0`
* **Port:** `8000` (diekspos melalui service)
* **Liveness Probe:** memeriksa respons HTTP pada jalur akar port 8000.

### Horizontal Pod Autoscaler

* **Nama:** `hpa-devops`
* **Target:** deployment `devops`
* **Scaling:**
    * Jumlah replika minimum: 1
    * Jumlah replika maksimum: 3
    * Skala berdasarkan penggunaan CPU:
        * Target: 80% penggunaan rata-rata

### Service

* **Nama:** `my-service`
* **Tipe:** `NodePort`
* **Selector:** `app: devops`
* **Pemetaan port:**
    * Port service: 8000
    * Port target: 8000 (port kontainer)
    * Port Node: 30007 (diekspos secara eksternal)

### Memulai

1. **Terapkan manifest:** Anda dapat menggunakan `kubectl apply -f` dengan setiap file manifest untuk mendeploy sumber daya.
2. **Akses service:** Service mengekspos aplikasi pada port `30007` dari setiap node di klaster Kubernetes Anda. Anda dapat menemukan alamat IP node dengan `kubectl get nodes`.
3. **Monitor autoscaler:** Gunakan `kubectl get hpa` untuk melihat skala dan metrik penggunaan sumber daya saat ini.
4. **Scaling manual:** Anda dapat menskala deployment secara manual dengan `kubectl scale deployments devops --replicas=X`, di mana X adalah jumlah replika yang diinginkan.

### Catatan tambahan:

* Liveness probe memastikan kontainer dalam keadaan sehat sebelum melayani lalu lintas.
* Anda dapat menyesuaikan metrik autoscaler dan perilaku scaling berdasarkan kebutuhan Anda.
* Ingat untuk memperbarui tag image dan nomor port jika diperlukan.

### Umpan
