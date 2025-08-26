<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>A la Vuelta — Prototipo de prueba</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    .chip { @apply inline-flex items-center px-2 py-0.5 rounded-full text-xs border; }
    .btn { @apply inline-flex items-center gap-2 px-3 py-2 rounded-xl shadow-sm border bg-white hover:bg-gray-50; }
    .btn-primary { @apply bg-green-600 text-white border-green-700 hover:bg-green-700; }
    .btn-ghost { @apply bg-transparent border-transparent hover:bg-gray-100; }
    .card { @apply bg-white rounded-2xl shadow p-4 border; }
    .input, select, textarea { @apply w-full border rounded-xl px-3 py-2 focus:outline-none focus:ring-2 focus:ring-green-500; }
    .hidden-admin { display: none !important; }
    .hidden { display: none !important; }
  </style>
</head>
<body class="bg-gray-50 text-gray-800">

<!-- Header -->
<header class="bg-white/90 backdrop-blur sticky top-0 z-40 border-b">
  <div class="max-w-6xl mx-auto px-4 py-3 flex items-center justify-between">
    <div class="flex items-center gap-3">
      <img src="https://placehold.co/96x96?text=AV" alt="Logo a la vuelta" class="w-12 h-12 object-contain">
      <div class="leading-tight">
        <div class="font-black text-xl">a la vuelta</div>
        <div class="text-sm text-gray-500">Ahorra, repara, conecta</div>
      </div>
    </div>

    <nav class="hidden md:flex items-center gap-2">
      <a href="#inicio" class="btn btn-ghost">Inicio</a>
      <a href="#catalogo" class="btn btn-ghost">Catálogo</a>
      <a href="#registro" class="btn btn-ghost">Registro</a>
      <a href="#membresia" class="btn btn-ghost">Membresía</a>
      <a href="#impacto" class="btn btn-ghost">Impacto</a>
    </nav>

    <div class="flex items-center gap-2">
      <button id="loginBtn" class="btn btn-primary">Iniciar sesión con Google</button>
      <button id="logoutBtn" class="btn hidden">Cerrar sesión</button>
    </div>
  </div>
</header>

<!-- Hero -->
<section id="inicio" class="max-w-6xl mx-auto px-4 py-10 grid md:grid-cols-2 gap-8">
  <div class="flex flex-col justify-center">
    <h1 class="text-3xl md:text-4xl font-black mb-4 text-green-700">Plataforma de intercambio comunitario</h1>
    <p class="mb-3">Conecta con tu comunidad para intercambiar, vender o donar artículos y servicios de segunda mano.</p>
    <ul class="list-disc list-inside mb-4">
      <li>Ahorrar dinero y reutilizar objetos</li>
      <li>Conectar con vecinos y compañeros</li>
      <li>Subir productos fácilmente con imágenes</li>
    </ul>
    <div class="flex gap-3">
      <a href="#catalogo" class="btn btn-primary">Ver catálogo</a>
    </div>
  </div>
  <div class="aspect-video bg-white rounded-2xl border shadow grid place-items-center p-8">
    <div id="memberCard" class="w-full max-w-md card hidden">
      <div class="flex items-center gap-3 mb-3">
        <img src="https://placehold.co/80x80?text=AV" alt="Logo" class="w-10 h-10 object-contain">
        <div class="font-bold" id="memberName">Socio Demo</div>
      </div>
      <div class="grid grid-cols-2 gap-3 text-sm">
        <div><span class="text-gray-500">Correo</span><div class="font-semibold" id="memberEmail">demo@correo.com</div></div>
        <div><span class="text-gray-500">N.º socio</span><div class="font-semibold">AV-001</div></div>
        <div><span class="text-gray-500">Beneficio</span><div class="font-semibold">10% reparaciones</div></div>
        <div><span class="text-gray-500">Estado</span><div class="font-semibold">Activo</div></div>
      </div>
    </div>
  </div>
</section>

<!-- Catálogo -->
<section id="catalogo" class="max-w-6xl mx-auto px-4 py-10">
  <div class="flex items-center justify-between mb-4">
    <h2 class="text-2xl font-black text-gray-900">Catálogo</h2>
    <button id="addItemBtn" class="btn btn-primary hidden">Añadir artículo</button>
  </div>

  <div class="grid sm:grid-cols-2 md:grid-cols-3 xl:grid-cols-4 gap-5" id="grid"></div>
  <div id="emptyState" class="hidden text-center text-gray-500 mt-8">No hay artículos.</div>
</section>

<!-- Registro / Subida de producto -->
<dialog id="itemModal" class="rounded-2xl p-0 w-full max-w-xl">
  <form method="dialog" class="card !rounded-2xl">
    <div class="flex items-center justify-between mb-4">
      <h3 id="modalTitle" class="text-xl font-black">Nuevo artículo</h3>
      <button class="btn">Cerrar</button>
    </div>
    <div class="grid md:grid-cols-2 gap-4">
      <div>
        <label class="text-sm text-gray-600">Título</label>
        <input id="fTitulo" class="input" required />
      </div>
      <div>
        <label class="text-sm text-gray-600">Estado/condición</label>
        <input id="fEstado" class="input" placeholder="Usado, nuevo, buen estado…" />
      </div>
      <div class="md:col-span-2">
        <label class="text-sm text-gray-600">Descripción</label>
        <textarea id="fDescripcion" class="input" rows="3"></textarea>
      </div>
      <div>
        <label class="text-sm text-gray-600">Precio (USD)</label>
        <input id="fPrecio" type="number" class="input" step="0.01" min="0" />
      </div>
      <div>
        <label class="text-sm text-gray-600">Método</label>
        <select id="fMetodo" class="input">
          <option value="venta">Venta</option>
          <option value="intercambio">Intercambio</option>
          <option value="gratis">Gratis</option>
        </select>
      </div>
      <div>
        <label class="text-sm text-gray-600">URL de foto</label>
        <input type="file" id="fFotoFile" class="input" accept="image/*"/>
      </div>
    </div>
    <div class="mt-5 flex justify-end gap-2">
      <button id="saveItemBtn" type="button" class="btn btn-primary">Guardar</button>
    </div>
  </form>
</dialog>

<!-- Membresía -->
<section id="membresia" class="max-w-6xl mx-auto px-4 py-10">
  <h2 class="text-2xl font-black mb-4">Membresía Premium</h2>
  <div class="grid md:grid-cols-2 gap-6">
    <div class="card">
      <h3 class="font-bold mb-2">Beneficios</h3>
      <ul class="list-disc list-inside space-y-1">
        <li>10% de descuento en reparaciones</li>
        <li>Acceso anticipado a ferias y eventos</li>
        <li>Promociones exclusivas</li>
      </ul>
    </div>
    <div class="card">
      <h3 class="font-bold mb-2">Calendario de eventos</h3>
      <div id="calendar" class="border rounded-xl p-4">Aquí se verán los eventos próximos</div>
    </div>
  </div>
</section>

<!-- Impacto -->
<section id="impacto" class="max-w-6xl mx-auto px-4 py-10">
  <h2 class="text-2xl font-black mb-4">Impacto estimado</h2>
  <div class="grid md:grid-cols-3 gap-4">
    <div class="card text-center">
      <div class="text-sm text-gray-500">Aceptación de membresía</div>
      <div id="kpiAceptacion" class="text-3xl font-black text-green-700">25%</div>
    </div>
    <div class="card text-center">
      <div class="text-sm text-gray-500">Ahorro estimado</div>
      <div id="kpiAhorro" class="text-3xl font-black text-green-700">$85</div>
    </div>
    <div class="card text-center">
      <div class="text-sm text-gray-500">Artículos publicados</div>
      <div id="kpiItems" class="text-3xl font-black text-green-700">0</div>
    </div>
  </div>
</section>

<footer class="border-t bg-white py-6 mt-10">
  <div class="max-w-6xl mx-auto px-4 text-sm text-gray-500 text-center">
    © 2025 a la vuelta — Prototipo de prueba
  </div>
</footer>

<script>
  // ====== Datos ======
  let items = [];
  let user = null;

  const grid = document.getElementById('grid');
  const emptyState = document.getElementById('emptyState');
  const addItemBtn = document.getElementById('addItemBtn');
  const itemModal = document.getElementById('itemModal');
  const fTitulo = document.getElementById('fTitulo');
  const fEstado = document.getElementById('fEstado');
  const fDescripcion = document.getElementById('fDescripcion');
  const fPrecio = document.getElementById('fPrecio');
  const fMetodo = document.getElementById('fMetodo');
  const fFotoFile = document.getElementById('fFotoFile');
  const saveItemBtn = document.getElementById('saveItemBtn');
  const memberCard = document.getElementById('memberCard');
  const memberName = document.getElementById('memberName');
  const memberEmail = document.getElementById('memberEmail');
  const loginBtn = document.getElementById('loginBtn');
  const logoutBtn = document.getElementById('logoutBtn');
  const kpiItems = document.getElementById('kpiItems');

  // ====== Funciones ======
  function renderCatalog(){
    grid.innerHTML = '';
    if(items.length===0){ emptyState.classList.remove('hidden'); kpiItems.textContent = 0; return; }
    emptyState.classList.add('hidden');
    kpiItems.textContent = items.length;
    items.forEach(it=>{
      const div = document.createElement('div');
      div.className='card flex flex-col';
      div.innerHTML=`
        <img src="${it.foto}" alt="${it.titulo}" class="rounded-xl h-40 w-full object-cover mb-3">
        <div class="flex-1">
          <div class="flex items-center justify-between gap-2 mb-1">
            <h3 class="font-bold line-clamp-1">${it.titulo}</h3>
            <span class="chip ${it.metodo==='venta'?'bg-green-100 text-green-700 border-green-300':it.metodo==='intercambio'?'bg-blue-100 text-blue-700 border-blue-300':'bg-gray-100 text-gray-700 border-gray-300'}">${it.metodo.charAt(0).toUpperCase()+it.metodo.slice(1)}</span>
          </div>
          <div class="text-sm text-gray-600 line-clamp-2">${it.descripcion||''}</div>
          <div class="mt-2 font-extrabold text-green-700">${it.precio>0?`$${it.precio}`:'—'}</div>
          <div class="text-xs text-gray-500">${it.estado||''}</div>
        </div>
      `;
      grid.appendChild(div);
    });
  }

  // ====== Simular login Google ======
  loginBtn.addEventListener('click', ()=>{
    user = { name:'Usuario Prueba', email:'prueba@correo.com' };
    memberName.textContent = user.name;
    memberEmail.textContent = user.email;
    memberCard.classList.remove('hidden');
    loginBtn.classList.add('hidden');
    logoutBtn.classList.remove('hidden');
    addItemBtn.classList.remove('hidden');
  });

  logoutBtn.addEventListener('click', ()=>{
    user = null;
    memberCard.classList.add('hidden');
    loginBtn.classList.remove('hidden');
    logoutBtn.classList.add('hidden');
    addItemBtn.classList.add('hidden');
  });

  // ====== Agregar artículo ======
  addItemBtn.addEventListener('click', ()=>{
    fTitulo.value=fEstado.value=fDescripcion.value=fPrecio.value=fMetodo.value='';
    fFotoFile.value='';
    itemModal.showModal();
  });

  saveItemBtn.addEventListener('click', ()=>{
    if(!fTitulo.value){ alert('Título obligatorio'); return; }
    const file = fFotoFile.files[0];
    if(!file){ alert('Selecciona una imagen'); return; }
    const reader = new FileReader();
    reader.onload = function(e){
      items.push({
        titulo: fTitulo.value,
        estado: fEstado.value,
        descripcion: fDescripcion.value,
        precio: Number(fPrecio.value||0),
        metodo: fMetodo.value||'venta',
        foto: e.target.result
      });
      renderCatalog();
      itemModal.close();
    };
    reader.readAsDataURL(file);
  });

  renderCatalog();
</script>

</body>
</html>
