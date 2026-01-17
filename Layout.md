import React from 'react';
import Navbar from './components/layout/Navbar';

export default function Layout({ children, currentPageName }) {
  return (
    <div className="min-h-screen bg-gradient-to-br from-green-50 via-white to-emerald-50">
      <Navbar />
      <main className="pt-16">
        {children}
      </main>
      
      {/* Footer */}
      <footer className="bg-green-900 text-white mt-20">
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-12">
          <div className="grid grid-cols-1 md:grid-cols-4 gap-8">
            <div className="col-span-1 md:col-span-2">
              <div className="text-2xl font-bold mb-4">HENS Pupuk Organik</div>
              <p className="text-green-100 mb-4">
                Solusi pupuk organik cair terbaik untuk hasil panen maksimal
              </p>
              <p className="text-sm text-green-200">
                © 2025 ANG Indonesia. All rights reserved.
              </p>
            </div>
            
            <div>
              <h3 className="font-semibold mb-4">Menu</h3>
              <ul className="space-y-2 text-green-100">
                <li><a href="#" className="hover:text-white transition-colors">Beranda</a></li>
                <li><a href="#" className="hover:text-white transition-colors">Produk</a></li>
                <li><a href="#" className="hover:text-white transition-colors">Panduan</a></li>
                <li><a href="#" className="hover:text-white transition-colors">Kontak</a></li>
              </ul>
            </div>
            
            <div>
              <h3 className="font-semibold mb-4">Bantuan</h3>
              <ul className="space-y-2 text-green-100">
                <li><a href="#" className="hover:text-white transition-colors">FAQ</a></li>
                <li><a href="#" className="hover:text-white transition-colors">Cara Penggunaan</a></li>
                <li><a href="#" className="hover:text-white transition-colors">Hubungi Kami</a></li>
              </ul>
            </div>
          </div>
        </div>
      </footer>
    </div>
  );
}
