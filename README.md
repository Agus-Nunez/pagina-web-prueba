export default App;
const App: React.FC = () => {
     // ... código ...
   };
   import React, { useState } from 'react';

// Definición de tipos
interface Producto {
  id: number;
  nombre: string;
  precio: number;
  categoria: string;
  descripcion: string;
}

interface ItemCarrito {
  producto: Producto;
  cantidad: number;
}

// Datos de ejemplo para productos
const productos: Producto[] = [
  {
    id: 1,
    nombre: "Chocolate",
    precio: 2.50,
    categoria: "dulces",
    descripcion: "Chocolate de leche de 100g"
  },
  {
    id: 2,
    nombre: "Gaseosa Cola",
    precio: 3.00,
    categoria: "bebidas",
    descripcion: "Gaseosa de cola de 500ml"
  },
  {
    id: 3,
    nombre: "Papas Fritas",
    precio: 2.00,
    categoria: "snacks",
    descripcion: "Bolsa de papas fritas saladas"
  },
  {
    id: 4,
    nombre: "Agua Mineral",
    precio: 1.50,
    categoria: "bebidas",
    descripcion: "Botella de agua mineral 500ml"
  },
  {
    id: 5,
    nombre: "Galletas",
    precio: 1.80,
    categoria: "dulces",
    descripcion: "Paquete de galletas variadas"
  },
  {
    id: 6,
    nombre: "Barra Energética",
    precio: 2.20,
    categoria: "snacks",
    descripcion: "Barra energética con frutos secos"
  }
];

// Componente de Producto
const ProductoCard: React.FC<{ producto: Producto; onAddToCart: (producto: Producto, cantidad: number) => void }> = ({ producto, onAddToCart }) => {
  const [cantidad, setCantidad] = useState(1);

  const handleAddToCart = () => {
    if (cantidad > 0) {
      onAddToCart(producto, cantidad);
      setCantidad(1); // Resetear cantidad después de añadir
    }
  };

  return (
    <div className="bg-white rounded-lg shadow-md p-4 flex flex-col">
      <div className="flex justify-center mb-4">
        <img 
          src="https://placeholder-image-service.onrender.com/image/150x150?prompt=Producto de kiosco&id=f289fa22-3467-4413-85c6-69f0a911f2f4" 
          alt={`Imagen de ${producto.nombre}`} 
          className="w-32 h-32 object-cover rounded"
        />
      </div>
      <h3 className="text-lg font-semibold text-foreground">{producto.nombre}</h3>
      <p className="text-sm text-muted-foreground mb-2">{producto.descripcion}</p>
      <p className="text-primary font-bold text-xl mb-4">${producto.precio.toFixed(2)}</p>
      
      <div className="flex items-center justify-between mb-4">
        <label htmlFor={`cantidad-${producto.id}`} className="text-sm font-medium text-foreground">
          Cantidad:
        </label>
        <input
          id={`cantidad-${producto.id}`}
          type="number"
          min="1"
          value={cantidad}
          onChange={(e) => setCantidad(Math.max(1, parseInt(e.target.value) || 1))}
          className="w-20 px-2 py-1 border border-border rounded text-foreground bg-background"
        />
      </div>
      
      <button
        onClick={handleAddToCart}
        className="bg-primary hover:bg-primary/90 text-primary-foreground font-medium py-2 px-4 rounded transition-colors"
      >
        Añadir al carrito
      </button>
    </div>
  );
};

// Componente de Carrito
const Carrito: React.FC<{ items: ItemCarrito[]; onRemoveItem: (id: number) => void; onCheckout: (metodo: 'web' | 'whatsapp') => void }> = ({ 
  items, 
  onRemoveItem, 
  onCheckout 
}) => {
  const total = items.reduce((sum, item) => sum + (item.producto.precio * item.cantidad), 0);

  if (items.length === 0) {
    return (
      <div className="bg-muted p-6 rounded-lg text-center">
        <p className="text-muted-foreground">El carrito está vacío</p>
      </div>
    );
  }

  return (
    <div className="bg-muted p-6 rounded-lg">
      <h2 className="text-xl font-semibold text-foreground mb-4">Tu Carrito</h2>
      
      <div className="space-y-4 mb-6">
        {items.map((item) => (
          <div key={item.producto.id} className="flex justify-between items-center bg-background p-3 rounded">
            <div>
              <h4 className="font-medium text-foreground">{item.producto.nombre}</h4>
              <p className="text-sm text-muted-foreground">
                {item.cantidad} x ${item.producto.precio.toFixed(2)}
              </p>
            </div>
            <div className="flex items-center space-x-2">
              <span className="font-semibold text-foreground">
                ${(item.producto.precio * item.cantidad).toFixed(2)}
              </span>
              <button
                onClick={() => onRemoveItem(item.producto.id)}
                className="text-destructive hover:text-destructive/80 text-sm"
              >
                Eliminar
              </button>
            </div>
          </div>
        ))}
      </div>
      
      <div className="border-t border-border pt-4 mb-6">
        <div className="flex justify-between items-center mb-4">
          <span className="text-lg font-semibold text-foreground">Total:</span>
          <span className="text-xl font-bold text-primary">${total.toFixed(2)}</span>
        </div>
      </div>
      
      <div className="space-y-3">
        <button
          onClick={() => onCheckout('web')}
          className="w-full bg-primary hover:bg-primary/90 text-primary-foreground font-medium py-3 px-4 rounded transition-colors"
        >
          Pagar ahora
        </button>
        
        <button
          onClick={() => onCheckout('whatsapp')}
          className="w-full bg-green-600 hover:bg-green-700 text-white font-medium py-3 px-4 rounded transition-colors flex items-center justify-center"
        >
          <svg className="w-5 h-5 mr-2" fill="currentColor" viewBox="0 0 24 24">
            <path d="M17.472 14.382c-.297-.149-1.758-.867-2.03-.967-.273-.099-.471-.148-.67.15-.197.297-.767.966-.94 1.164-.173.199-.347.223-.644.075-.297-.15-1.255-.463-2.39-1.475-.883-.788-1.48-1.761-1.653-2.059-.173-.297-.018-.458.13-.606.134-.133.298-.347.446-.52.149-.174.198-.298.298-.497.099-.198.05-.371-.025-.52-.075-.149-.669-1.612-.916-2.207-.242-.579-.487-.5-.67-.51-.173-.008-.371-.01-.57-.01-.198 0-.52.074-.792.372-.272.297-1.04 1.016-1.04 2.479 0 1.462 1.065 2.875 1.213 3.074.149.198 2.096 3.2 5.077 4.487.709.306 1.262.489 1.694.625.712.227 1.36.195 1.871.118.571-.085 1.758-.719 2.006-1.413.248-.694.248-1.289.173-1.413-.074-.124-.272-.198-.57-.347m-5.446 7.443c-.987.003-1.897-.198-2.713-.594-1.263-.594-2.23-1.585-2.917-2.817-.496-.889-.826-1.877-.97-2.913-.074-.52-.074-1.04 0-1.56.074-.52.248-1.04.496-1.523.248-.483.57-.93.967-1.34.397-.41.862-.744 1.387-.992.525-.248 1.09-.41 1.683-.483.594-.074 1.189-.05 1.758.074.57.124 1.115.372 1.609.744.496.371.92.843 1.263 1.388.347.545.595 1.141.744 1.758.149.619.198 1.262.149 1.906-.05.644-.198 1.289-.496 1.906-.297.619-.694 1.189-1.189 1.683-.496.496-1.091.892-1.758 1.189-.669.297-1.388.446-2.133.446"/>
          </svg>
          Pagar por WhatsApp
        </button>
      </div>
    </div>
  );
};

// Componente Principal
const KioscoApp: React.FC = () => {
  const [pestañaActiva, setPestañaActiva] = useState<'inicio' | 'productos'>('inicio');
  const [carrito, setCarrito] = useState<ItemCarrito[]>([]);
  const [categoriaSeleccionada, setCategoriaSeleccionada] = useState<string>('todos');

  // Obtener categorías únicas
  const categorias = ['todos', ...new Set(productos.map(p => p.categoria))];

  // Filtrar productos por categoría
  const productosFiltrados = categoriaSeleccionada === 'todos' 
    ? productos 
    : productos.filter(p => p.categoria === categoriaSeleccionada);

  const añadirAlCarrito = (producto: Producto, cantidad: number) => {
    setCarrito(prev => {
      const existingItem = prev.find(item => item.producto.id === producto.id);
      if (existingItem) {
        return prev.map(item =>
          item.producto.id === producto.id
            ? { ...item, cantidad: item.cantidad + cantidad }
            : item
        );
      }
      return [...prev, { producto, cantidad }];
    });
  };

  const eliminarDelCarrito = (id: number) => {
    setCarrito(prev => prev.filter(item => item.producto.id !== id));
  };

  const handleCheckout = (metodo: 'web' | 'whatsapp') => {
    if (metodo === 'whatsapp') {
      const mensaje = encodeURIComponent(
        `Hola! Quiero realizar el siguiente pedido:\n${carrito.map(item => 
          `${item.cantidad}x ${item.producto.nombre} - $${(item.producto.precio * item.cantidad).toFixed(2)}`
        ).join('\n')}\n\nTotal: $${carrito.reduce((sum, item) => sum + (item.producto.precio * item.cantidad), 0).toFixed(2)}`
      );
      window.open(`https://wa.me/5491112345678?text=${mensaje}`, '_blank');
    } else {
      alert('Procesando pago... (esta funcionalidad se implementaría con una pasarela de pago real)');
      // Aquí iría la integración con la pasarela de pago
    }
    setCarrito([]);
  };

  return (
    <div className="min-h-screen bg-background text-foreground">
      {/* Header */}
      <header className="bg-primary text-primary-foreground py-4 px-6">
        <div className="container mx-auto">
          <h1 className="text-2xl font-bold">Kiosco El Buen Sabor</h1>
          <p className="text-primary-foreground/80">Los mejores productos al mejor precio</p>
        </div>
      </header>

      {/* Navegación */}
      <nav className="bg-muted border-b border-border">
        <div className="container mx-auto px-6">
          <div className="flex space-x-1">
            <button
              onClick={() => setPestañaActiva('inicio')}
              className={`px-4 py-3 font-medium transition-colors ${
                pestañaActiva === 'inicio'
                  ? 'bg-background text-foreground border-b-2 border-primary'
                  : 'text-muted-foreground hover:text-foreground'
              }`}
            >
              Inicio
            </button>
            <button
              onClick={() => setPestañaActiva('productos')}
              className={`px-4 py-3 font-medium transition-colors ${
                pestañaActiva === 'productos'
                  ? 'bg-background text-foreground border-b-2 border-primary'
                  : 'text-muted-foreground hover:text-foreground'
              }`}
            >
              Productos
            </button>
          </div>
        </div>
      </nav>

      {/* Contenido Principal */}
      <main className="container mx-auto px-6 py-8">
        {pestañaActiva === 'inicio' && (
          <div className="space-y-8">
            <section className="text-center">
              <h2 className="text-3xl font-bold text-foreground mb-4">Bienvenido a Nuestro Kiosco</h2>
              <p className="text-muted-foreground text-lg max-w-2xl mx-auto">
                Ofrecemos una amplia variedad de productos frescos y de calidad. 
                Desde dulces y snacks hasta bebidas refrescantes, todo en un solo lugar.
              </p>
            </section>

            <section className="grid md:grid-cols-3 gap-6">
              <div className="bg-muted p-6 rounded-lg text-center">
                <div className="w-16 h-16 bg-primary/20 rounded-full flex items-center justify-center mx-auto mb-4">
                  <svg className="w-8 h-8 text-primary" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M12 8v13m0-13V6a2 2 0 112 2h-2zm0 0V5.5A2.5 2.5 0 109.5 8H12zm-7 4h14M5 12a2 2 0 110-4h14a2 2 0 110 4M5 12v7a2 2 0 002 2h10a2 2 0 002-2v-7" />
                  </svg>
                </div>
                <h3 className="text-xl font-semibold text-foreground mb-2">Productos Frescos</h3>
                <p className="text-muted-foreground">Ofrecemos productos de la mejor calidad y siempre frescos.</p>
              </div>

              <div className="bg-muted p-6 rounded-lg text-center">
                <div className="w-16 h-16 bg-primary/20 rounded-full flex items-center justify-center mx-auto mb-4">
                  <svg className="w-8 h-8 text-primary" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M17 9V7a2 2 0 00-2-2H5a2 2 0 00-2 2v6a2 2 0 002 2h2m2 4h10a2 2 0 002-2v-6a2 2 0 00-2-2H9a2 2 0 00-2 2v6a2 2 0 002 2zm7-5a2 2 0 11-4 0 2 2 0 014 0z" />
                  </svg>
                </div>
                <h3 className="text-xl font-semibold text-foreground mb-2">Envío Rápido</h3>
                <p className="text-muted-foreground">Realizamos entregas rápidas en toda la zona.</p>
              </div>

              <div className="bg-muted p-6 rounded-lg text-center">
                <div className="w-16 h-16 bg-primary/20 rounded-full flex items-center justify-center mx-auto mb-4">
                  <svg className="w-8 h-8 text-primary" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M4.318 6.318a4.5 4.5 0 000 6.364L12 20.364l7.682-7.682a4.5 4.5 0 00-6.364-6.364L12 7.636l-1.318-1.318a4.5 4.5 0 00-6.364 0z" />
                  </svg>
                </div>
                <h3 className="text-xl font-semibold text-foreground mb-2">Atención Personalizada</h3>
                <p className="text-muted-foreground">Nos preocupamos por brindarte la mejor experiencia de compra.</p>
              </div>
            </section>
          </div>
        )}

        {pestañaActiva === 'productos' && (
          <div className="grid lg:grid-cols-4 gap-8">
            {/* Filtros y Carrito */}
            <div className="lg:col-span-1 space-y-6">
              {/* Filtro de Categorías */}
              <div className="bg-muted p-4 rounded-lg">
                <h3 className="font-semibold text-foreground mb-3">Categorías</h3>
                <div className="space-y-2">
                  {categorias.map(categoria => (
                    <button
                      key={categoria}
                      onClick={() => setCategoriaSeleccionada(categoria)}
                      className={`block w-full text-left px-3 py-2 rounded transition-colors ${
                        categoriaSeleccionada === categoria
                          ? 'bg-primary text-primary-foreground'
                          : 'hover:bg-muted/50 text-foreground'
                      }`}
                    >
                      {categoria.charAt(0).toUpperCase() + categoria.slice(1)}
                    </button>
                  ))}
                </div>
              </div>

              {/* Carrito */}
              <Carrito 
                items={carrito} 
                onRemoveItem={eliminarDelCarrito}
                onCheckout={handleCheckout}
              />
            </div>

            {/* Lista de Productos */}
            <div className="lg:col-span-3">
              <h2 className="text-2xl font-bold text-foreground mb-6">
                {categoriaSeleccionada === 'todos' ? 'Todos los Productos' : `Productos de ${categoriaSeleccionada}`}
              </h2>
              
              <div className="grid md:grid-cols-2 lg:grid-cols-3 gap-6">
                {productosFiltrados.map(producto => (
                  <ProductoCard
                    key={producto.id}
                    producto={producto}
                    onAddToCart={añadirAlCarrito}
                  />
                ))}
              </div>
            </div>
          </div>
        )}
      </main>

      {/* Footer */}
      <footer className="bg-muted border-t border-border py-6 mt-12">
        <div className="container mx-auto px-6 text-center">
          <p className="text-muted-foreground">
            © 2024 Kiosco El Buen Sabor. Todos los derechos reservados.
          </p>
          <p className="text-muted-foreground text-sm mt-2">
            Horario: Lunes a Domingo 8:00 - 22:00
          </p>
        </div>
      </footer>
    </div>
  );
};

export default App;
