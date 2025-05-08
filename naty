""import { useState } from 'react';
import { Card, CardContent } from '@/components/ui/card';
import { Button } from '@/components/ui/button';
import { Input } from '@/components/ui/input';
import { createClient } from '@supabase/supabase-js';

const supabaseUrl = 'https://<TU_SUPABASE_URL>'; // Aquí debes colocar tu URL de Supabase
const supabaseAnonKey = '<TU_SUPABASE_ANON_KEY>'; // Aquí tu API Key pública
const supabase = createClient(supabaseUrl, supabaseAnonKey);

const carpetas = [
  { nombre: 'Notas Internas Enviadas - DG' },
  { nombre: 'Notas Internas Enviadas - DAF' },
  { nombre: 'Notas Internas Enviadas - UPC' },
  { nombre: 'Notas Internas Recibidas - DG' },
  { nombre: 'Notas Internas Recibidas - DAF' },
  { nombre: 'Notas Internas Recibidas - UPC' },
  { nombre: 'Informes Semanales' },
  { nombre: 'Informes Mensuales' },
  { nombre: 'Comisiones Bancarias' },
  { nombre: 'Arqueo de Caja' },
  { nombre: 'Otros Aportes' },
  { nombre: 'Movimientos Económicos' }
];

export default function Home() {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [error, setError] = useState('');
  const [isAuthenticated, setIsAuthenticated] = useState(false);
  const [selectedFolder, setSelectedFolder] = useState(null);
  const [files, setFiles] = useState([]);
  const [file, setFile] = useState(null);

  const handleLogin = async (e) => {
    e.preventDefault();
    const { error } = await supabase.auth.signInWithPassword({
      email,
      password,
    });

    if (error) {
      setError(error.message);
    } else {
      setIsAuthenticated(true);
    }
  };

  const handleOpenFolder = async (folder) => {
    setSelectedFolder(folder);
    const { data, error } = await supabase.storage.from('documentos').list(folder.nombre);

    if (!error) {
      setFiles(data);
    }
  };

  const handleFileUpload = async () => {
    if (file && selectedFolder) {
      const { error } = await supabase.storage
        .from('documentos')
        .upload(`${selectedFolder.nombre}/${file.name}`, file);

      if (!error) {
        alert('Archivo subido correctamente');
        handleOpenFolder(selectedFolder); // Refresca la lista
      } else {
        alert('Error al subir archivo');
      }
    }
  };

  const handleFileDownload = async (fileName) => {
    const { data, error } = await supabase.storage
      .from('documentos')
      .download(`${selectedFolder.nombre}/${fileName}`);

    if (data) {
      const url = URL.createObjectURL(data);
      const link = document.createElement('a');
      link.href = url;
      link.setAttribute('download', fileName);
      document.body.appendChild(link);
      link.click();
      link.parentNode.removeChild(link);
    } else {
      alert('Error al descargar el archivo');
    }
  };

  const handleFileDelete = async (fileName) => {
    const { error } = await supabase.storage
      .from('documentos')
      .remove([`${selectedFolder.nombre}/${fileName}`]);

    if (!error) {
      alert('Archivo eliminado correctamente');
      handleOpenFolder(selectedFolder); // Refresca la lista
    } else {
      alert('Error al eliminar el archivo');
    }
  };

  return (
    <div className="min-h-screen flex items-center justify-center bg-gray-100">
      {isAuthenticated ? (
        <div className="grid grid-cols-2 gap-4 p-5">
          {selectedFolder ? (
            <Card className="p-4 w-full">
              <CardContent>
                <h3 className="text-lg font-semibold mb-4">{selectedFolder.nombre}</h3>
                <input type="file" onChange={(e) => setFile(e.target.files[0])} />
                <Button className="mt-2 w-full" onClick={handleFileUpload}>Subir Archivo</Button>

                <div className="mt-4">
                  <h4 className="font-bold mb-2">Archivos:</h4>
                  {files.map((file, index) => (
                    <div key={index} className="flex justify-between items-center mb-2">
                      <p className="text-sm">{file.name}</p>
                      <div className="space-x-2">
                        <Button onClick={() => handleFileDownload(file.name)}>Descargar</Button>
                        <Button onClick={() => handleFileDelete(file.name)} className="bg-red-500">Eliminar</Button>
                      </div>
                    </div>
                  ))}
                </div>

                <Button className="mt-4 w-full" onClick={() => setSelectedFolder(null)}>Volver</Button>
              </CardContent>
            </Card>
          ) : (
            carpetas.map((carpeta, index) => (
              <Card key={index} className="p-4">
                <CardContent>
                  <h3 className="text-lg font-semibold">{carpeta.nombre}</h3>
                  <Button className="mt-2 w-full" onClick={() => handleOpenFolder(carpeta)}>Abrir</Button>
                </CardContent>
              </Card>
            ))
          )}
        </div>
      ) : (
        <Card className="w-full max-w-md p-5">
          <CardContent>
            <h2 className="text-2xl font-bold mb-4 text-center">Iniciar Sesión</h2>
            {error && <p className="text-red-500">{error}</p>}
            <form onSubmit={handleLogin} className="space-y-4">
              <Input 
                type="email" 
                placeholder="Correo electrónico" 
                value={email} 
                onChange={(e) => setEmail(e.target.value)} 
              />
              <Input 
                type="password" 
                placeholder="Contraseña" 
                value={password} 
                onChange={(e) => setPassword(e.target.value)} 
              />
              <Button type="submit" className="w-full">Ingresar</Button>
            </form>
          </CardContent>
        </Card>
      )}
    </div>
  );
}""
