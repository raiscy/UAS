# UAS

# Tugas UAS Bahasa Pemrograman
Buatlah program sederhana dengan ketentuan:
1. Program harus dibuat dengan konsep modular dan oop (pisahkan class data, class view, dan class process)
2. Program harus meminta input dari pengguna
3. Tambahkan validasi input (dapat menggunakan konsep eksepsi)
4. Program menampilkan hasil (dapat berupa table view)

# KODE PROGRAM 

    class Product:
        def __init__(self, code, name, price, stock):
            self.code = code
            self.name = name
            self.price = price
            self.stock = stock


    class Cart:
        def __init__(self):
            self.items = {}

        def add_to_cart(self, product, quantity):
            if product.stock < quantity:
                print(f"Stok tidak cukup! Stok tersedia: {product.stock}")
                return

            if product.code in self.items:
                self.items[product.code]['quantity'] += quantity
            else:
                self.items[product.code] = {'product': product, 'quantity': quantity}

            product.stock -= quantity
            print(f"{product.name} berhasil ditambahkan ke keranjang.")

      def remove_from_cart(self, code):
          if code in self.items:
            item = self.items.pop(code)
            item['product'].stock += item['quantity']
            print(f"{item['product'].name} berhasil dihapus dari keranjang.")
        else:
            print("Produk tidak ditemukan di keranjang.")

    def view_cart(self):
        if not self.items:
            print("Keranjang kosong.")
        else:
            print("\n--- Keranjang Belanja ---")
            for item in self.items.values():
                product = item['product']
                quantity = item['quantity']
                print(f"{product.code} - {product.name} - {quantity} x Rp{product.price} = Rp{product.price * quantity}")

    def calculate_total(self):
        total = 0
        for item in self.items.values():
            total += item['product'].price * item['quantity']
        return total


    class Cashier:
        def __init__(self, cart):
            self.cart = cart
            self.transaction_history = []

      def checkout(self):
          total = self.cart.calculate_total()
          if total == 0:
              print("Keranjang kosong. Tidak ada yang bisa di-checkout.")
              return

          print("\n--- Checkout ---")
          self.cart.view_cart()
          print(f"Total harga: Rp{total}")
          discount = int(input("Masukkan diskon (%): "))
          discount_amount = (total * discount) // 100
          total_after_discount = total - discount_amount
          print(f"Diskon: Rp{discount_amount}")
          print(f"Total setelah diskon: Rp{total_after_discount}")

          while True:
            payment = int(input("Masukkan jumlah uang: Rp"))
            if payment >= total_after_discount:
                change = payment - total_after_discount
                print(f"Pembayaran berhasil! Kembalian: Rp{change}")
                self.transaction_history.append({
                    'items': self.cart.items.copy(),
                    'total': total_after_discount,
                    'discount': discount_amount
                })
                self.cart.items.clear()
                break
            else:
                print(f"Uang tidak cukup. Kurang: Rp{total_after_discount - payment}")

    def print_receipt(self):
        if not self.transaction_history:
            print("Belum ada transaksi yang selesai.")
            return

        print("\n--- Cetak Struk ---")
        last_transaction = self.transaction_history[-1]
        for item in last_transaction['items'].values():
            product = item['product']
            quantity = item['quantity']
            print(f"{product.code} - {product.name} - {quantity} x Rp{product.price} = Rp{product.price * quantity}")
        print("----------------------")
        print(f"Diskon: Rp{last_transaction['discount']}")
        print(f"Total Bayar: Rp{last_transaction['total']}")
        print("======================")

    def view_transaction_history(self):
        if not self.transaction_history:
            print("Belum ada transaksi.")
            return

        print("\n--- Riwayat Transaksi ---")
        for idx, transaction in enumerate(self.transaction_history, start=1):
            print(f"Transaksi #{idx}:")
            for item in transaction['items'].values():
                product = item['product']
                quantity = item['quantity']
                print(f"  {product.name} - {quantity} x Rp{product.price}")
            print(f"  Total: Rp{transaction['total']} (Diskon: Rp{transaction['discount']})")
            print("----------------------")


    def main():
        # Data produk awal
        product_list = [
            Product("001", "Cupcakes", 30000, 10),
            Product("002", "Redvelvet", 45000, 20),
            Product("003", "Macaron", 18000, 15),
            Product("004", "Cinnamon Roll", 23000, 30),
            Product("005", "Milkshake", 20000, 10),
            Product("006", "Icecream", 25000, 10),
        ]

        cart = Cart()
        cashier = Cashier(cart)

    while True:
        print("\n=== Welcome To Raifa's Bakery ===")
        print("1. Lihat Menu")
        print("2. Tambahkan Menu ke Keranjang")
        print("3. Lihat Keranjang")
        print("4. Hapus Menu dari Keranjang")
        print("5. Checkout dan Pembayaran")
        print("6. Cetak Struk Belanja")
        print("7. Lihat Riwayat Transaksi")
        print("8. Keluar")
        choice = input("Pilih opsi diatas untuk melanjutkan: ")

        if choice == "1":
            print("\n--- Raifa's Bakery Menu List ---")
            for product in product_list:
                print(f"{product.code} - {product.name} - Rp{product.price} (Stok: {product.stock})")
        elif choice == "2":
            code = input("Masukkan kode menu: ")
            quantity = int(input("Masukkan jumlah: "))
            selected_product = next((p for p in product_list if p.code == code), None)
            if selected_product:
                cart.add_to_cart(selected_product, quantity)
            else:
                print("Produk tidak ditemukan.")
        elif choice == "3":
            cart.view_cart()
        elif choice == "4":
            code = input("Masukkan kode produk yang ingin dihapus: ")
            cart.remove_from_cart(code)
        elif choice == "5":
            cashier.checkout()
        elif choice == "6":
            cashier.print_receipt()
        elif choice == "7":
            cashier.view_transaction_history()
        elif choice == "8":
            print("Terima kasih telah berbelanja di Raifa's Bakery. Semoga hidup kalian tetap manis seperti kue yang dijual disini!")
            break
        else:
            print("Pilihan tidak valid. Coba lagi.")


    if __name__ == "__main__":
        main()
