<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Generator Modul Ajar IPAS (Sistem Organ)</title>
    <!-- Load Tailwind CSS untuk styling modern dan responsive -->
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Inter', sans-serif; background-color: #0d1117; color: #c9d1d9; }
        .card { background-color: #161b22; border: 1px solid #30363d; }
        input[type="text"], textarea { background-color: #0d1117; border: 1px solid #30363d; color: #c9d1d9; }
        .section-header { border-bottom: 2px solid #30363d; padding-bottom: 0.5rem; margin-bottom: 1rem; color: #58a6ff; font-weight: 700; }
        /* Style untuk output DOCX */
        .doc-output { white-space: pre-wrap; font-size: 0.9rem; background-color: #0d1117; padding: 1.5rem; border-radius: 0.5rem; }
        .loading-spinner { border-top-color: #58a6ff; -webkit-animation: spinner 1s linear infinite; animation: spinner 1s linear infinite; }
        @-webkit-keyframes spinner { 0% { -webkit-transform: rotate(0deg); } 100% { -webkit-transform: rotate(360deg); } }
        @keyframes spinner { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }
    </style>
    <!-- Library untuk generate DOCX (menggunakan FileSaver.js) -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/FileSaver.js/2.0.5/FileSaver.js"></script>
</head>
<body class="p-4 sm:p-8">
    <div class="max-w-4xl mx-auto">
        <h1 class="text-3xl font-bold text-center mb-6 text-white">
            🧬 Generator Modul Ajar IPAS (Sistem Organ)
        </h1>
        <p class="text-center text-sm mb-8 text-gray-400">
            Tempelkan teks Modul Ajar Anda, lalu tekan **"Auto-Parse"** untuk mengisi formulir. Gunakan tombol **Generate Varian** untuk membuat konten baru!
        </p>

        <!-- QUICK PASTE INPUT -->
        <div class="card p-6 rounded-xl shadow-lg mb-8">
            <h2 class="text-xl section-header">1. Tempel Teks Modul Ajar Sampel</h2>
            <p class="text-sm text-gray-400 mb-3">Salin dan tempelkan semua teks dari dokumen Modul Ajar Anda di sini. (Modul Ajar IPAS Anda sudah terdeteksi memiliki format yang baik untuk Auto-Parse).</p>
            <textarea id="rawInput" rows="10" class="w-full p-3 rounded-lg" placeholder="Tempelkan seluruh teks Modul Ajar Anda di sini..."></textarea>
            <button onclick="parseAndFill()" class="w-full bg-yellow-600 hover:bg-yellow-700 text-white font-bold py-3 px-6 rounded-xl shadow-lg transition duration-300 mt-4">
                ✨ AUTO-PARSE DAN ISI FORMULIR
            </button>
        </div>

        <!-- INPUT FORM -->
        <div id="inputForm" class="space-y-6 card p-6 rounded-xl shadow-lg">

            <!-- INFORMASI UMUM -->
            <div class="mb-8">
                <h2 class="text-xl section-header">2. INFORMASI UMUM (Hasil Parse)</h2>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <input type="text" id="namaGuru" placeholder="Nama Guru" class="w-full p-2 rounded-lg" value="">
                    <input type="text" id="satuanPendidikan" placeholder="Satuan Pendidikan" class="w-full p-2 rounded-lg" value="">
                    <input type="text" id="jenjangKelas" placeholder="Jenjang / Kelas" class="w-full p-2 rounded-lg" value="">
                    <input type="text" id="tahunPelajaran" placeholder="Tahun Pelajaran" class="w-full p-2 rounded-lg" value="">
                </div>
                <input type="text" id="mataPelajaran" placeholder="Mata Pelajaran (Topik Modul Ajar)" class="w-full p-2 rounded-lg mt-4" value="">
                <input type="text" id="alokasiWaktu" placeholder="Alokasi Waktu" class="w-full p-2 rounded-lg mt-4" value="">
            </div>

            <!-- B. DESAIN PEMBELAJARAN -->
            <div class="mb-8">
                <h2 class="text-xl section-header">B. DESAIN PEMBELAJARAN</h2>
                <label class="block mb-2 text-sm font-medium">TUJUAN PEMBELAJARAN</label>
                <div class="flex space-x-2">
                    <textarea id="tujuan" rows="3" class="flex-grow p-2 rounded-lg" placeholder="Contoh: Mengidentifikasi fungsi sistem organ dan menganalisis keterkaitan pola hidup sehat."></textarea>
                    <button id="btnTujuan" onclick="generateVariant('tujuan', 'Buatkan 3 varian Tujuan Pembelajaran untuk topik Sistem Organ Tubuh Manusia, Fokuskan pada keterampilan menganalisis dan menciptakan.', 2)" class="w-1/4 bg-purple-600 hover:bg-purple-700 text-white font-bold p-2 rounded-lg transition duration-300 flex items-center justify-center">
                        <span id="txtTujuan">Generate Varian</span>
                    </button>
                </div>

                <label class="block mt-4 mb-2 text-sm font-medium">PERTANYAAN PEMANTIK</label>
                <div class="flex space-x-2">
                    <textarea id="pemantik" rows="2" class="flex-grow p-2 rounded-lg" placeholder="Contoh: Mengapa kita bisa tersedak saat makan sambil berbicara?"></textarea>
                    <button id="btnPemantik" onclick="generateVariant('pemantik', 'Buatkan 5 varian Pertanyaan Pemantik (engaging questions) untuk memicu diskusi tentang kaitan sistem pernapasan dan peredaran darah.', 1)" class="w-1/4 bg-purple-600 hover:bg-purple-700 text-white font-bold p-2 rounded-lg transition duration-300 flex items-center justify-center">
                        <span id="txtPemantik">Generate Varian</span>
                    </button>
                </div>
                
                <label class="block mt-4 mb-2 text-sm font-medium">Model & Metode Pedagogik</label>
                <input type="text" id="pedagogik" placeholder="Model Pembelajaran dan Metode" class="w-full p-2 rounded-lg" value="">
            </div>

            <!-- C. LANGKAH-LANGKAH INTI (Fokus 3M) -->
            <div class="mb-8">
                <h2 class="text-xl section-header">C. LANGKAH-LANGKAH INTI (Fokus 3M)</h2>
                <p class="text-sm text-yellow-400 mb-2">Gunakan tombol Generate untuk mengubah aktivitas 3M (Memahami, Mengaplikasi, Merefleksi).</p>
                
                <label class="block mt-4 mb-1 text-sm font-medium">INTI - Memahami (Berkesadaran)</label>
                <div class="flex space-x-2">
                    <textarea id="memahami" rows="2" class="flex-grow p-2 rounded-lg" placeholder="Aktivitas untuk membangun konsep dasar"></textarea>
                    <button id="btnMemahami" onclick="generateVariant('memahami', 'Buatkan 2 varian aktivitas Memahami (Berkesadaran) untuk topik Sistem Ekskresi. Fokus pada media visual interaktif dan eksplorasi.', 1)" class="w-1/4 bg-purple-600 hover:bg-purple-700 text-white font-bold p-2 rounded-lg transition duration-300 flex items-center justify-center">
                        <span id="txtMemahami">Generate Varian</span>
                    </button>
                </div>

                <label class="block mt-4 mb-1 text-sm font-medium">INTI - Mengaplikasi (Bermakna)</label>
                <div class="flex space-x-2">
                    <textarea id="mengaplikasi" rows="2" class="flex-grow p-2 rounded-lg" placeholder="Aktivitas praktik atau proyek"></textarea>
                    <button id="btnMengaplikasi" onclick="generateVariant('mengaplikasi', 'Buatkan 2 varian aktivitas Mengaplikasi (Bermakna) yang berfokus pada proyek seni atau presentasi hasil analisis pola hidup sehat.', 1)" class="w-1/4 bg-purple-600 hover:bg-purple-700 text-white font-bold p-2 rounded-lg transition duration-300 flex items-center justify-center">
                        <span id="txtMengaplikasi">Generate Varian</span>
                    </button>
                </div>

                <label class="block mt-4 mb-1 text-sm font-medium">INTI - Merefleksi (Menggembirakan)</label>
                <div class="flex space-x-2">
                    <textarea id="merefleksi" rows="2" class="flex-grow p-2 rounded-lg" placeholder="Aktivitas untuk evaluasi diri dan umpan balik"></textarea>
                    <button id="btnMerefleksi" onclick="generateVariant('merefleksi', 'Buatkan 2 varian aktivitas Merefleksi (Menggembirakan) yang melibatkan teknologi atau permainan ringan (game-based assessment).', 1)" class="w-1/4 bg-purple-600 hover:bg-purple-700 text-white font-bold p-2 rounded-lg transition duration-300 flex items-center justify-center">
                        <span id="txtMerefleksi">Generate Varian</span>
                    </button>
                </div>
            </div>
            <!-- D. ASESMEN & E/F. PENUTUP -->
            <div class="mb-8">
                <h2 class="text-xl section-header">D. ASESMEN & PENUTUP</h2>
                <label class="block mb-2 text-sm font-medium">ASESMEN (Awal, Proses, Akhir)</label>
                <textarea id="asesmen" rows="3" class="w-full p-2 rounded-lg" placeholder="Contoh: Awal: Kuis pra-materi. Proses: Observasi keaktifan diskusi. Akhir: Tes Uraian (analisis pola hidup sehat)."></textarea>

                <label class="block mt-4 mb-2 text-sm font-medium">GLOSARIUM & DAFTAR PUSTAKA</label>
                <textarea id="penutup" rows="3" class="w-full p-2 rounded-lg" placeholder="Isi dengan istilah teknis dan sumber daya yang digunakan."></textarea>
            </div>

            <!-- Action Button -->
            <div class="flex flex-col sm:flex-row space-y-4 sm:space-y-0 sm:space-x-4">
                <button onclick="generateDocx()" class="flex-1 bg-green-600 hover:bg-green-700 text-white font-bold py-3 px-6 rounded-xl shadow-lg transition duration-300">
                    ⬇️ UNDUH MODUL Ajar (.DOCX)
                </button>
                <button onclick="previewContent()" class="flex-1 bg-blue-600 hover:bg-blue-700 text-white font-bold py-3 px-6 rounded-xl shadow-lg transition duration-300">
                    👁️ PREVIEW Konten
                </button>
            </div>
        </div>

        <!-- OUTPUT PREVIEW -->
        <div id="outputPreview" class="mt-8 card p-6 rounded-xl hidden">
            <h2 class="text-xl section-header">Preview Konten Modul Ajar</h2>
            <div id="docContent" class="doc-output"></div>
        </div>
    </div>

    <!-- JAVASCRIPT LOGIC -->
    <script>
        const API_KEY = ""; 
        const API_URL = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=${API_KEY}`;
        const MAX_RETRIES = 5;

        // Fungsi utilitas untuk mengekstrak konten antara dua kata kunci
        function extractContent(text, startKeyword, endKeyword, isOptional = false) {
            const startIndex = text.indexOf(startKeyword);
            if (startIndex === -1) {
                if (isOptional) return '';
                return `[ERROR: Keyword '${startKeyword}' not found for extraction.]`;
            }

            const subText = text.substring(startIndex + startKeyword.length).trim();
            const endIndex = endKeyword ? subText.indexOf(endKeyword) : -1;

            if (endIndex === -1 && endKeyword) {
                return subText.trim();
            } else if (endKeyword) {
                return subText.substring(0, endIndex).trim();
            } else {
                return subText.trim();
            }
        }

        // Fungsi Auto-Parse yang sudah diperbarui
        function parseAndFill() {
            const rawText = document.getElementById('rawInput').value;
            if (rawText.trim() === "") {
                alert("Silakan tempelkan teks Modul Ajar di kotak input terlebih dahulu.");
                return;
            }

            const normalizedText = rawText.replace(/\s+/g, ' ').replace(/(\r\n|\n|\r)/gm, ' ').trim();

            // 1. INFORMASI UMUM (Disempurnakan berdasarkan dokumen Anda)
            document.getElementById('satuanPendidikan').value = extractContent(normalizedText, 'Satuan Pendidikan : ', 'Nama Guru', true).replace(':', '').trim();
            document.getElementById('namaGuru').value = extractContent(normalizedText, 'Nama Guru : ', 'Jenjang / Kelas', true).replace(':', '').trim();
            document.getElementById('jenjangKelas').value = extractContent(normalizedText, 'Jenjang / Kelas : ', 'Mata Pelajaran', true).replace(':', '').trim();
            document.getElementById('mataPelajaran').value = extractContent(normalizedText, 'Mata Pelajaran : ', 'Alokasi Waktu', true).replace(':', '').trim();
            document.getElementById('alokasiWaktu').value = extractContent(normalizedText, 'Alokasi Waktu : ', 'Tahun Pelajaran', true).replace(':', '').trim();
            document.getElementById('tahunPelajaran').value = extractContent(normalizedText, 'Tahun Pelajaran : ', 'A. IDENTIFIKASI', true).replace(':', '').trim();

            // 2. IDENTIFIKASI
            const siswaSection = extractContent(normalizedText, 'IDENTIFIKASI PESERTA DIDIK', 'IDENTIFIKASI MATERI PEMBELAJARAN', true);
            document.getElementById('identifikasiSiswa').value = siswaSection.replace('• Minat Peserta Didik', 'Minat Peserta Didik:').replace('Latar Belakang', '\nLatar Belakang:').replace('• Kebutuhan Belajar', '\nKebutuhan Belajar:').trim();

            const materiSection = extractContent(normalizedText, 'IDENTIFIKASI MATERI PEMBELAJARAN', 'DIMENSI PROFIL LULUSAN', true);
            document.getElementById('identifikasiMateri').value = materiSection.replace('Jenis Pengetahuan', 'Jenis Pengetahuan:').replace('• Relevansi Tingkat Kesulitan', '\nRelevansi Kesulitan:').replace('Struktur Materi', '\nStruktur Materi:').trim();

            // 3. DESAIN PEMBELAJARAN
            document.getElementById('tujuan').value = extractContent(normalizedText, 'TUJUAN PEMBELAJARAN', 'MATERI PEMBELAJARAN', true).trim();
            document.getElementById('pemantik').value = extractContent(normalizedText, 'PERTANYAAN PEMANTIK', 'PRAKTEK PEDAGOGIK', true).trim();
            document.getElementById('pedagogik').value = extractContent(normalizedText, 'PRAKTEK PEDAGOGIK', 'Pembelajaran Berdiferensiasi', true).trim();


            // 4. LANGKAH-LANGKAH INTI (3M) - Fokus Pertemuan 1
            const intiSection = extractContent(normalizedText, 'PERTEMUAN 1 (70 menit)', 'PERTEMUAN 2 (70 menit)', true);
            document.getElementById('memahami').value = extractContent(intiSection, 'Memahami (berkesadaran):', 'Mengaplikasi (bermakna):', true).trim();
            document.getElementById('mengaplikasi').value = extractContent(intiSection, 'Mengaplikasi (bermakna):', 'Merefleksi (menggembirakan):', true).trim();
            document.getElementById('merefleksi').value = extractContent(intiSection, 'Merefleksi (menggembirakan):', 'Penutup (10 menit)', true).trim(); 


            // 5. ASESMEN & PENUTUP
            const asesmenSection = extractContent(normalizedText, 'D. ASSESMENT', 'E. GLOSARIUM', true);
            // Hanya mengambil bagian rubrik dan instrumen kunci, bukan seluruh soal
            document.getElementById('asesmen').value = "Rubrik Asesmen Awal:\n" + extractContent(asesmenSection, 'Rubrik Penskoran', 'ASESMEN AWAL', true).trim() + 
                                                      "\n\nRubrik Asesmen Proses:\n" + extractContent(asesmenSection, 'Instrumen Penilaian Poster Hidup Sehat', '3. ASESMEN AKHIR', true).trim();
            
            // Penutup (Glosarium & Pustaka)
            document.getElementById('penutup').value = extractContent(normalizedText, 'E. GLOSARIUM', 'F. DAFTAR PUSTAKA', true).trim() + 
                                                     "\n\nDAFTAR PUSTAKA:\n" + extractContent(normalizedText, 'F. DAFTAR PUSTAKA', 'Mengetahui,', true).trim();

            console.log("Data berhasil di-parse. Silakan periksa formulir.");
        }


        // Fungsi koneksi ke Gemini API dengan Exponential Backoff
        async function fetchWithRetry(payload, retries = 0) {
            try {
                const response = await fetch(API_URL, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });

                if (response.status === 429 && retries < MAX_RETRIES) {
                    const delay = Math.pow(2, retries) * 1000;
                    await new Promise(resolve => setTimeout(resolve, delay));
                    return fetchWithRetry(payload, retries + 1);
                }

                if (!response.ok) {
                    throw new Error(`API Error: ${response.statusText}`);
                }
                
                return await response.json();

            } catch (error) {
                if (retries < MAX_RETRIES) {
                    const delay = Math.pow(2, retries) * 1000;
                    await new Promise(resolve => setTimeout(resolve, delay));
                    return fetchWithRetry(payload, retries + 1);
                }
                throw new Error("Failed to connect to API after multiple retries.");
            }
        }

        // Fungsi Generate Varian Utama
        async function generateVariant(fieldId, prompt, type) {
            const inputElement = document.getElementById(fieldId);
            const buttonElement = document.getElementById(`btn${fieldId.charAt(0).toUpperCase() + fieldId.slice(1)}`);
            const buttonTextElement = document.getElementById(`txt${fieldId.charAt(0).toUpperCase() + fieldId.slice(1)}`);
            
            const originalContent = inputElement.value;

            // 1. Tampilkan status loading
            buttonElement.disabled = true;
            buttonElement.classList.remove('bg-purple-600', 'hover:bg-purple-700');
            buttonElement.classList.add('bg-gray-500');
            buttonTextElement.innerHTML = '<div class="loading-spinner h-5 w-5 border-4 border-gray-200 border-solid rounded-full"></div>';

            const systemPrompt = `Anda adalah ahli kurikulum Merdeka. Tugas Anda adalah menghasilkan konten Modul Ajar IPAS yang variatif dan kreatif. Berikan hasil Anda dalam format poin atau daftar yang mudah dibaca. JANGAN tambahkan kata pengantar atau penutup. Hanya berikan konten yang diminta.`;
            let userQuery = '';

            // Tentukan query berdasarkan tipe input
            if (originalContent && originalContent.trim().length > 10) {
                userQuery = `Berdasarkan input awal ini: "${originalContent}". ${prompt} Buatkan varian yang sepenuhnya berbeda dari input awal tersebut.`;
            } else {
                userQuery = `${prompt} Berikan konten yang sesuai dengan format Modul Ajar (IPAS Kelas VI, Sistem Organ).`;
            }

            const payload = {
                contents: [{ parts: [{ text: userQuery }] }],
                systemInstruction: {
                    parts: [{ text: systemPrompt }]
                },
                // Gunakan tools search untuk mendapatkan ide yang relevan dan terkini
                tools: [{ "google_search": {} }]
            };

            try {
                const result = await fetchWithRetry(payload);
                const generatedText = result.candidates?.[0]?.content?.parts?.[0]?.text || "Gagal menghasilkan konten baru. Coba lagi.";

                // 2. Tampilkan hasil
                inputElement.value = generatedText;
                console.log(`Successfully generated content for: ${fieldId}`);
                
            } catch (error) {
                console.error("Gemini API call failed:", error);
                alert("Gagal koneksi ke server AI. Coba lagi atau periksa koneksi internet Anda.");
                inputElement.value = originalContent + "\n\n[Gagal menghasilkan varian. Silakan coba lagi.]";
            } finally {
                // 3. Reset tombol
                buttonElement.disabled = false;
                buttonElement.classList.remove('bg-gray-500');
                buttonElement.classList.add('bg-purple-600', 'hover:bg-purple-700');
                buttonTextElement.textContent = 'Generate Varian';
            }
        }


        // Fungsi untuk mengambil data dari form
        function collectData() {
            return {
                namaGuru: document.getElementById('namaGuru').value,
                satuanPendidikan: document.getElementById('satuanPendidikan').value,
                jenjangKelas: document.getElementById('jenjangKelas').value,
                tahunPelajaran: document.getElementById('tahunPelajaran').value,
                mataPelajaran: document.getElementById('mataPelajaran').value,
                alokasiWaktu: document.getElementById('alokasiWaktu').value,
                identifikasiSiswa: document.getElementById('identifikasiSiswa').value,
                identifikasiMateri: document.getElementById('identifikasiMateri').value,
                tujuan: document.getElementById('tujuan').value,
                pemantik: document.getElementById('pemantik').value,
                pedagogik: document.getElementById('pedagogik').value,
                memahami: document.getElementById('memahami').value,
                mengaplikasi: document.getElementById('mengaplikasi').value,
                merefleksi: document.getElementById('merefleksi').value,
                asesmen: document.getElementById('asesmen').value,
                penutup: document.getElementById('penutup').value,
                tanggal: new Date().toLocaleDateString('id-ID', { day: 'numeric', month: 'long', year: 'numeric' })
            };
        }

        // Fungsi untuk menampilkan preview di layar
        function previewContent() {
            const data = collectData();
            const content = `
                --- PERENCANAAN PEMBELAJARAN MENDALAM ---

                **INFORMASI UMUM**
                Satuan Pendidikan: ${data.satuanPendidikan}
                Nama Guru: ${data.namaGuru}
                Jenjang / Kelas: ${data.jenjangKelas}
                Mata Pelajaran: ${data.mataPelajaran}
                Alokasi Waktu: ${data.alokasiWaktu}
                Tahun Pelajaran: ${data.tahunPelajaran}
                
                [Bagian A. IDENTIFIKASI diabaikan di sini karena sifatnya statis]

                **B. DESAIN PEMBELAJARAN**
                TUJUAN PEMBELAJARAN:
                ${data.tujuan}

                PERTANYAAN PEMANTIK:
                ${data.pemantik}

                PRAKTEK PEDAGOGIK:
                ${data.pedagogik}
                
                **C. LANGKAH-LANGKAH INTI (Pertemuan 1)**
                Inti (Fase 3M):
                - Memahami (Berkesadaran): ${data.memahami}
                - Mengaplikasi (Bermakna): ${data.mengaplikasi}
                - Merefleksi (Menggembirakan): ${data.merefleksi}
                
                **D. ASESMEN**
                ${data.asesmen}
                
                **E. GLOSARIUM & F. DAFTAR PUSTAKA**
                ${data.penutup}
                
                [Generated on: ${data.tanggal}]
            `;

            document.getElementById('docContent').textContent = content;
            document.getElementById('outputPreview').classList.remove('hidden');
        }

        // Fungsi utama untuk Generate dan Download DOC
        function generateDocx() {
            const data = collectData();
            
            if (!data.memahami || !data.mengaplikasi || !data.merefleksi) {
                console.error("Langkah Inti (Memahami, Mengaplikasi, Merefleksi) harus diisi lengkap!");
                return;
            }

            // Konten DOC (Menggunakan format Markdown/Teks kaya untuk diubah menjadi .doc)
            const docContent = `
PERENCANAAN PEMBELAJARAN MENDALAM
================================

### INFORMASI UMUM
| Keterangan | Detail |
| :--- | :--- |
| **Satuan Pendidikan** | ${data.satuanPendidikan} |
| **Nama Guru** | ${data.namaGuru} |
| **Jenjang / Kelas** | ${data.jenjangKelas} |
| **Mata Pelajaran** | ${data.mataPelajaran} |
| **Alokasi Waktu** | ${data.alokasiWaktu} |
| **Tahun Pelajaran** | ${data.tahunPelajaran} |

[Tambahkan Bagian A. IDENTIFIKASI secara manual jika diperlukan, atau isi di form]

### B. DESAIN PEMBELAJARAN
**TUJUAN PEMBELAJARAN:**
${data.tujuan}

**PERTANYAAN PEMANTIK:**
${data.pemantik}

**PRAKTEK PEDAGOGIK (Model & Metode):**
${data.pedagogik}

---

### C. LANGKAH-LANGKAH INTI (Pertemuan 1)

**Inti (Fase 3M)**

* **Memahami (Berkesadaran):** ${data.memahami}
* **Mengaplikasi (Bermakna):** ${data.mengaplikasi}
* **Merefleksi (Menggembirakan):** ${data.merefleksi}

---

### D. ASESMEN
${data.asesmen}

### E. GLOSARIUM & F. DAFTAR PUSTAKA
${data.penutup}

[Generated on: ${data.tanggal}]
            `;
            
            // Mengunduh file dengan ekstensi .doc
            const filename = `Modul_Ajar_IPAS_Sistem_Organ_Varian.doc`;
            const blob = new Blob([docContent], {
                type: "application/msword;charset=utf-8"
            });
            saveAs(blob, filename);

            document.getElementById('docContent').textContent = docContent;
            document.getElementById('outputPreview').classList.remove('hidden');
        }
    </script>
</body>
</html>
