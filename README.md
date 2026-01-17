import React, { useEffect, useState } from 'react';
import { base44 } from '@/api/base44Client';
import { createPageUrl } from '@/utils';
import { useMutation, useQueryClient } from '@tanstack/react-query';
import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card';
import { Button } from '@/components/ui/button';
import { Input } from '@/components/ui/input';
import { Label } from '@/components/ui/label';
import { Select, SelectContent, SelectItem, SelectTrigger, SelectValue } from '@/components/ui/select';
import { Textarea } from '@/components/ui/textarea';
import { Loader2, MapPin } from 'lucide-react';

const provinces = [
  'Aceh', 'Sumatera Utara', 'Sumatera Barat', 'Riau', 'Jambi', 'Sumatera Selatan',
  'Bengkulu', 'Lampung', 'Kepulauan Bangka Belitung', 'Kepulauan Riau',
  'DKI Jakarta', 'Jawa Barat', 'Jawa Tengah', 'DI Yogyakarta', 'Jawa Timur',
  'Banten', 'Bali', 'Nusa Tenggara Barat', 'Nusa Tenggara Timur',
  'Kalimantan Barat', 'Kalimantan Tengah', 'Kalimantan Selatan', 'Kalimantan Timur', 'Kalimantan Utara',
  'Sulawesi Utara', 'Sulawesi Tengah', 'Sulawesi Selatan', 'Sulawesi Tenggara', 'Gorontalo', 'Sulawesi Barat',
  'Maluku', 'Maluku Utara', 'Papua', 'Papua Barat', 'Papua Selatan', 'Papua Tengah', 'Papua Pegunungan', 'Papua Barat Daya'
];

export default function AddFarm() {
  const queryClient = useQueryClient();
  const [user, setUser] = useState(null);
  const [formData, setFormData] = useState({
    farm_name: '',
    owner_name: '',
    address: '',
    city: '',
    province: '',
    area_m2: '',
    phone: '',
    status: 'active'
  });

  const createFarmMutation = useMutation({
    mutationFn: (data) => base44.entities.Farm.create(data),
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['farms'] });
      window.location.href = createPageUrl('MyFarms');
    }
  });

  useEffect(() => {
    loadUser();
  }, []);

  const loadUser = async () => {
    try {
      const currentUser = await base44.auth.me();
      setUser(currentUser);
      setFormData(prev => ({ ...prev, owner_name: currentUser.full_name }));
    } catch (error) {
      window.location.href = createPageUrl('Home');
    }
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    const dataToSubmit = {
      ...formData,
      area_m2: parseFloat(formData.area_m2)
    };
    createFarmMutation.mutate(dataToSubmit);
  };

  if (!user) return null;

  return (
    <div className="min-h-screen py-8">
      <div className="max-w-3xl mx-auto px-4 sm:px-6 lg:px-8">
        <Card>
          <CardHeader>
            <div className="flex items-center gap-3 mb-2">
              <div className="w-12 h-12 bg-green-100 rounded-xl flex items-center justify-center">
                <MapPin className="w-6 h-6 text-green-600" />
              </div>
              <div>
                <CardTitle className="text-2xl">Tambah Lahan Baru</CardTitle>
                <p className="text-gray-600 text-sm">Lengkapi data lahan pertanian Anda</p>
              </div>
            </div>
          </CardHeader>

          <CardContent>
            <form onSubmit={handleSubmit} className="space-y-6">
              <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
                <div className="md:col-span-2">
                  <Label htmlFor="farm_name">Nama Lahan *</Label>
                  <Input
                    id="farm_name"
                    placeholder="Contoh: Sawah Belakang Rumah"
                    value={formData.farm_name}
                    onChange={(e) => setFormData({ ...formData, farm_name: e.target.value })}
                    required
                    className="mt-1"
                  />
                </div>

                <div>
                  <Label htmlFor="owner_name">Nama Pemilik *</Label>
                  <Input
                    id="owner_name"
                    placeholder="Nama lengkap pemilik"
                    value={formData.owner_name}
                    onChange={(e) => setFormData({ ...formData, owner_name: e.target.value })}
                    required
                    className="mt-1"
                  />
                </div>

                <div>
                  <Label htmlFor="phone">Nomor Telepon</Label>
                  <Input
                    id="phone"
                    type="tel"
                    placeholder="08xxxxxxxxxx"
                    value={formData.phone}
                    onChange={(e) => setFormData({ ...formData, phone: e.target.value })}
                    className="mt-1"
                  />
                </div>

                <div className="md:col-span-2">
                  <Label htmlFor="address">Alamat Lengkap</Label>
                  <Textarea
                    id="address"
                    placeholder="Jalan, RT/RW, Desa/Kelurahan"
                    value={formData.address}
                    onChange={(e) => setFormData({ ...formData, address: e.target.value })}
                    className="mt-1"
                    rows={3}
                  />
                </div>

                <div>
                  <Label htmlFor="city">Kota/Kabupaten *</Label>
                  <Input
                    id="city"
                    placeholder="Contoh: Bandung"
                    value={formData.city}
                    onChange={(e) => setFormData({ ...formData, city: e.target.value })}
                    required
                    className="mt-1"
                  />
                </div>

                <div>
                  <Label htmlFor="province">Provinsi *</Label>
                  <Select value={formData.province} onValueChange={(value) => setFormData({ ...formData, province: value })} required>
                    <SelectTrigger className="mt-1">
                      <SelectValue placeholder="Pilih provinsi" />
                    </SelectTrigger>
                    <SelectContent>
                      {provinces.map((prov) => (
                        <SelectItem key={prov} value={prov}>
                          {prov}
                        </SelectItem>
                      ))}
                    </SelectContent>
                  </Select>
                </div>

                <div>
                  <Label htmlFor="area_m2">Luas Lahan (m²) *</Label>
                  <Input
                    id="area_m2"
                    type="number"
                    placeholder="Contoh: 5000"
                    value={formData.area_m2}
                    onChange={(e) => setFormData({ ...formData, area_m2: e.target.value })}
                    required
                    min="1"
                    step="0.01"
                    className="mt-1"
                  />
                  {formData.area_m2 && (
                    <p className="text-sm text-gray-600 mt-1">
                      ≈ {(parseFloat(formData.area_m2) / 10000).toFixed(4)} hektar
                    </p>
                  )}
                </div>
              </div>

              <div className="bg-blue-50 border border-blue-200 rounded-lg p-4">
                <p className="text-sm text-blue-900">
                  <strong>Catatan:</strong> Luas lahan digunakan untuk menghitung dosis pupuk. 
                  Panduan dalam buku adalah untuk 1 hektar (10,000 m²). Sistem akan otomatis menyesuaikan dosis.
                </p>
              </div>

              <div className="flex gap-4 pt-4">
                <Button
                  type="button"
                  variant="outline"
                  onClick={() => window.history.back()}
                  className="flex-1"
                >
                  Batal
                </Button>
                <Button
                  type="submit"
                  disabled={createFarmMutation.isPending}
                  className="flex-1 bg-green-600 hover:bg-green-700"
                >
                  {createFarmMutation.isPending ? (
                    <>
                      <Loader2 className="w-4 h-4 mr-2 animate-spin" />
                      Menyimpan...
                    </>
                  ) : (
                    'Simpan Lahan'
                  )}
                </Button>
              </div>
            </form>
          </CardContent>
        </Card>
      </div>
    </div>
  );
}
