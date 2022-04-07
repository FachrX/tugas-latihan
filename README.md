#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <malloc.h>

typedef struct {
	char kode[10];
	char nama[10];
	int stok;
	int harga;
}barang;

typedef struct elmt *alamat;

typedef struct elmt {
	barang br;
	alamat next;
}elemen;

typedef struct {
	elemen* first;
	elemen* tail;
}list;

void createList (list *L) {
	(*L).first=NULL;
	(*L).tail=NULL;
}

int countElemen (list L) {
	int jumlah = 0;
	if (L.first != NULL) {
		elemen* now = L.first;
		while (now != NULL) {
			jumlah ++;
			now=now->next;
		}
	}
	return jumlah;
}

void addFirst (list *L, char kode[], char nama[], int stok, int harga) {

	elemen *baru = (elemen*)malloc(sizeof(elemen));
	strcpy(baru->br.kode, kode);
	strcpy(baru->br.nama, nama);
	baru->br.stok = stok ;
	baru->br.harga = harga;
	if ((*L).first == NULL) {
		(*L).first = baru;
		(*L).tail = baru;
		baru->next = NULL;
	} else {
		baru->next = (*L).first;
		(*L).first = baru;
	}
}

void addLast (list *L, char kode[], char nama[], int stok, int harga) {
	if ((*L).first != NULL) {
		elemen *baru = (elemen*)malloc(sizeof(elemen));
	
		strcpy(baru->br.kode, kode);
		strcpy(baru->br.nama, nama);
		baru->br.stok = stok ;
		baru->br.harga = harga;
		
		baru->next = NULL;
		(*L).tail->next = baru;
		(*L).tail = NULL;
	} else {
		addFirst(L,kode,nama,stok,harga);
	}
}

void addAfter (list *L, int urutan, char kode[], char nama[], int stok, int harga) {
	if (urutan<=countElemen(*L)) {
		elemen *baru = (elemen*)malloc(sizeof(elemen));
		elemen *sebelum = (*L).first;
		int j=1;
		while (j<urutan) {
			sebelum = sebelum->next;
			j++;
		}
		if (sebelum != NULL) {
			strcpy(baru->br.kode, kode);
			strcpy(baru->br.nama, nama);
			baru->br.stok = stok ;
			baru->br.harga = harga;
			if (sebelum == (*L).tail) {
				sebelum->next = baru;
				baru->next =NULL;
				(*L).tail = baru;
			} else {
				baru->next = sebelum->next;
				sebelum->next = baru;
			}
		} else {
			addFirst(L,kode,nama,stok,harga);
		}
	} else {
		printf ("elemen tidak ada\n");
	}
}

void delFirst (list *L) {
	if ((*L).first != NULL) {
		if (countElemen(*L) == 1) {
			elemen *elhapus = (*L).first;
			(*L).first = NULL;
			(*L).tail = NULL;
			free (elhapus);
		} else {
			elemen *elhapus = (*L).first;
			(*L).first = elhapus->next;
			elhapus->next = NULL;
			free (elhapus);
		}
	} else {
		printf ("List masih kosong\n");
	}
}

void delLast(list *L) {
	if ((*L).first != NULL) {
		if (countElemen(*L) == 1) {
			delFirst(L);
		} else {
			elemen *terakhir = (*L).first;
			elemen *sterakhir = NULL;
			
			while (terakhir->next != NULL) {
				sterakhir = terakhir;
				terakhir = terakhir->next;
			}
			
			terakhir->next = NULL;
			(*L).tail = sterakhir;
			sterakhir->next = NULL;
			free (terakhir);
		}
	} else {
		printf ("List masih kosong\n");
	}
}

void delAfter(list *L, int urutan) {
	if ((*L).first != NULL) {
		if (urutan < countElemen(*L)) {
			if (countElemen(*L) == 1) {
				delFirst(L);
			} else {
				int c=1;
				
				elemen *hapus = (*L).first;
				elemen *shapus = NULL;
				
				while (c <= urutan) {
					shapus = hapus;
					hapus = hapus->next;
					c++;
				}
				
				shapus->next = hapus->next;
				hapus->next = NULL;
				free (hapus);
			}
		} else if (urutan == countElemen(*L)) {
			delLast(L);
		} else {
			printf ("tidak ada urutannya\n");
		}
	} else {
		printf ("List masih kosong\n");
	}
}
			
void findElemen(list L, char cari[]) {
	if (L.first != NULL) {
		int ketemu = 0;
		elemen *elmt = L.first;
		while(elmt != NULL) {
			if (strcmp(elmt->br.kode, cari) || strcmp(elmt->br.nama, cari)) {
				
				printf ("\n-----------------------------------------\n");
				printf ("Kode Barang   : %s\n", elmt->br.kode);
				printf ("Nama Barang   : %s\n", elmt->br.nama);
				printf ("Stok          : %d\n", elmt->br.stok);
				printf ("Harga(satuan) : %d\n", elmt->br.harga);
				printf ("Total Harga   : %d\n", elmt->br.harga*elmt->br.stok);
				printf ("-----------------------------------------\n");
				ketemu += 1;
			}
			elmt = elmt->next ;
		}
		if (ketemu == 0) {
			printf("Data yang dicari tidak ada\n");
		} 
	} else {
			printf ("List masih kosong\n");
		}
}

void printElemen(list L) {
	if (L.first != NULL) {
		elemen *elmt = L.first;
		int i = 1;
		while (elmt != NULL) {
			printf ("Elemen Ke - %d\n", i);
			printf ("Kode Barang   : %s\n", elmt->br.kode);
			printf ("Nama Barang   : %s\n", elmt->br.nama);
			printf ("Stok          : %d\n", elmt->br.stok);
			printf ("Harga(satuan) : %d\n", elmt->br.harga);
			printf ("Total Harga   : %d\n", elmt->br.harga*elmt->br.stok);
			printf ("-----------------------------------------\n");
			elmt = elmt->next;
			i += 1;
		}
	} else {
		printf ("List masih kosong\n");
	}
}

int main () {
	list L;
	int pilih, pilih1, pilih2;
	int jml, stok, harga, uradd, urdel;
	int exit = 0;
	char kode[10];
	char nama[10];
	char cari[10];
	createList(&L);
	
	awal:
	do{
		jml = countElemen(L);
		system ("cls");
		printf("=========================================\n");
		printf("|\t Selamat Datang Di IndoAlfa  \t|");
		printf("\n=========================================\n");
		printf(" Banyak Data Saat Ini : %d\n\n", jml);
		printf(" Menu :\n");
		printf(" 1. Tambah Data\n");
		printf(" 2. Hapus Data\n");
		printf(" 3. Cari Data\n");
		printf(" 4. Tampilkan Data\n");
		printf(" 5. Keluar\n\n");
		printf(" Ketikan Pilihan [1-5] : "); scanf("%d", &pilih);
	
		switch (pilih) {
			case 1 :
				system("cls");
				printf(" Tambah Data\n");
				printf("================================\n\n");
				printf(" Menu : \n");
				printf(" 1. Add First\n");
				printf(" 2. Add After\n");
				printf(" 3. Add Last\n");
				printf(" 4. Kembali\n\n");
				printf("Ketikan Pilihan [1-4] : "); scanf("%d", &pilih1);
	
				switch (pilih1) {
					case 1 :
						printf ("Masukan Kode Barang  : "); 
						fflush(stdin);
						gets(kode);
						printf ("Masukan Nama Barang  : "); 
						fflush(stdin);
						gets(nama);
						printf ("Masukan Stok         : "); scanf("%d", &stok);
						printf ("Masukan Harga Satuan : "); scanf("%d", &harga);
					
						addFirst(&L,kode,nama,stok,harga);
						printf("\nData berhasil disimpan\n");
						system ("pause");
						break;
					case 2 :
						printf ("Masukan Urutan Setelah Data : "); scanf ("%d", &uradd);
						if(uradd==0){
							printf ("Masukan Kode Barang  : "); 
							fflush(stdin);
							gets(kode);
							printf ("Masukan Nama Barang  : "); 
							fflush(stdin);
							gets(nama);
							printf ("Masukan Stok         : "); scanf("%d", &stok);
							printf ("Masukan Harga Satuan : "); scanf("%d", &harga);
						
							addFirst(&L,kode,nama,stok,harga);
							printf("\nData berhasil disimpan\n");
						} else {		
							if(jml>=uradd) {
								printf ("Masukan Kode Barang  : "); 
								fflush(stdin);
								gets(kode);
								printf ("Masukan Nama Barang  : "); 
								fflush(stdin);
								gets(nama);
								printf ("Masukan Stok         : "); scanf("%d", &stok);
								printf ("Masukan Harga Satuan : "); scanf("%d", &harga);
								
								addAfter(&L,uradd,kode,nama,stok,harga);
								printf("\nData berhasil disimpan\n");	
							} else {
								printf("urutannya tidak ada\n");
							}
						}			
						system("pause");
						break;
					case 3 :
						printf ("Masukan Kode Barang  : "); 
						fflush(stdin);
						gets(kode);
						printf ("Masukan Nama Barang  : "); 
						fflush(stdin);
						gets(nama);
						printf ("Masukan Stok         : "); scanf("%d", &stok);
						printf ("Masukan Harga Satuan : "); scanf("%d", &harga);
					
						addLast(&L,kode,nama,stok,harga);
						printf("\nData berhasil disimpan\n");
						system ("pause");
						break;
					case 4 :
						goto awal;
					default :
						printf ("Maaf Pilihan Anda Tidak Tersedia\n");
						printf ("Coba Ulang Kembali\n");
						system ("pause");
						break;
				}
				break;
			case 2 :
				system("cls");
				printf(" Hapus Data\n");
				printf("================================\n\n");
				printf(" Menu : \n");
				printf(" 1. Del First\n");
				printf(" 2. Del After\n");
				printf(" 3. Del Last\n");
				printf(" 4. Kembali\n\n");
				printf("Ketikan Pilihan [1-4] : "); scanf("%d", &pilih2);
				
				switch (pilih2) {
					case 1 :
						delFirst(&L);
						printf("Data Telah Terhapus\n");
						system ("pause");
						break;
					case 2 :
						printf("Hapus Data Setelah Urutan : "); scanf ("%d", &urdel);
						
						if(urdel <= jml) {
							delAfter(&L, urdel);
							printf("Data Telah Terhapus\n");
						} else if (urdel == 0) {
							delFirst(&L);
							printf("Data Telah Terhapus\n");
							} 
						else {
							printf("Urutannya tidak ada\n");
						}
						system("pause");
						break;
					case 3 :
						delLast(&L);
						printf("Data Telah Terhapus\n");
						system ("pause");
						break;
					case 4 :
						goto awal;
					default :
						printf ("Maaf Pilihan Anda Tidak Tersedia\n");
						printf ("Coba Ulang Kembali\n");
						system ("pause");
						break;
				}
				break;
			case 3 :
				system("cls");
				printf(" Cari Data Produk\n");
				printf("================================\n");
				printf ("Masukan Kode/Nama Barang  : ");
				fflush(stdin);
				gets(cari);
				findElemen(L,cari);
				system("pause");
				break;
			case 4 :
				system ("cls");
				printf(" Data Produk\n");
				printf("================================\n\n");
				printElemen(L);
				system ("pause");
				break;
			case 5 :
				system ("cls");
				printf("\nApakah anda ingin keluar dari program ?\n");
				printf("Iya (1) atau Tidak (0) : "); scanf("%d", &exit); 
				break;
			default :
				printf ("Maaf Pilihan Anda Tidak Tersedia\n");
				printf ("Coba Ulang Kembali\n");
				system ("pause");
				break;
		}
	} while (exit != 1);
	return 0;
}	
