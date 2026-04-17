<script setup>
import { ref, computed } from 'vue'

const props = defineProps({
  products: {
    type: Array,
    default: () => []
  }
})

const emit = defineEmits(['save-order', 'back'])

const productSearch = ref('')
const cart = ref([])

const form = ref({
  clientName: '',
  clientPhone: '',
  clientAddress: '',
  clientNotes: ''
})

const filteredProducts = computed(() => {
  const search = productSearch.value.trim().toLowerCase()

  if (!search) return props.products

  return props.products.filter(product =>
    product.name.toLowerCase().includes(search) ||
    product.code.toLowerCase().includes(search)
  )
})

const subtotal = computed(() =>
  cart.value.reduce((acc, item) => acc + item.price * item.quantity, 0)
)

const tax = computed(() => subtotal.value * 0.15)
const total = computed(() => subtotal.value + tax.value)

function addToCart(product) {
  const existing = cart.value.find(item => item.id === product.id)

  if (existing) {
    existing.quantity++
  } else {
    cart.value.push({ ...product, quantity: 1 })
  }
}

function changeQuantity(productId, amount) {
  const item = cart.value.find(product => product.id === productId)
  if (!item) return

  item.quantity += amount

  if (item.quantity <= 0) {
    cart.value = cart.value.filter(product => product.id !== productId)
  }
}

function removeFromCart(productId) {
  cart.value = cart.value.filter(item => item.id !== productId)
}

function clearOrder() {
  form.value = {
    clientName: '',
    clientPhone: '',
    clientAddress: '',
    clientNotes: ''
  }
  cart.value = []
  productSearch.value = ''
}

function saveOrder() {
  const clientName = form.value.clientName.trim()

  if (!clientName) {
    alert('Ingrese el nombre del cliente.')
    return
  }

  if (cart.value.length === 0) {
    alert('Agregue al menos un producto.')
    return
  }

  emit('save-order', {
    clientName,
    clientPhone: form.value.clientPhone.trim(),
    clientAddress: form.value.clientAddress.trim(),
    clientNotes: form.value.clientNotes.trim(),
    items: [...cart.value],
    itemsCount: cart.value.reduce((acc, item) => acc + item.quantity, 0),
    subtotal: subtotal.value,
    tax: tax.value,
    total: total.value
  })

  clearOrder()
}
</script>

<template>
  <section>
    <div class="order-shell">
      <div class="content-card premium-order-card">
        <div class="order-header">
          <div>
            <span class="section-badge">Operaciones</span>
            <h2>Nuevo Pedido</h2>
            <p>Registra los datos del cliente y agrega productos al carrito.</p>
          </div>

          <button class="secondary" @click="emit('back')">← Volver</button>
        </div>

        <div class="pedido-form-layout">
          <!-- COLUMNA IZQUIERDA -->
          <div class="pedido-left premium-panel">
            <div class="panel-title">
              <h3>Datos del cliente</h3>
            </div>

            <div class="form-grid">
              <div class="form-group">
                <label>Nombre del cliente</label>
                <input v-model="form.clientName" type="text" placeholder="Ingrese el nombre">
              </div>

              <div class="form-group">
                <label>Teléfono</label>
                <input v-model="form.clientPhone" type="text" placeholder="Ingrese el teléfono">
              </div>
            </div>

            <div class="form-group">
              <label>Dirección</label>
              <input v-model="form.clientAddress" type="text" placeholder="Ingrese la dirección">
            </div>

            <div class="form-group">
              <label>Observación</label>
              <textarea v-model="form.clientNotes" rows="3" placeholder="Notas adicionales"></textarea>
            </div>

            <div class="panel-title top-space">
              <h3>Productos</h3>
            </div>

            <div class="search-box premium-search">
              <input v-model="productSearch" type="text" placeholder="Buscar producto por nombre o código">
            </div>

            <div class="product-list premium-product-list">
              <div class="product-card premium-product-card" v-for="product in filteredProducts" :key="product.id">
                <div class="product-main">
                  <strong>{{ product.name }}</strong>
                  <small>Código: {{ product.code }}</small>
                </div>

                <div class="product-side">
                  <span class="product-price">${{ product.price.toFixed(2) }}</span>
                  <button @click="addToCart(product)">Agregar</button>
                </div>
              </div>

              <p v-if="filteredProducts.length === 0" class="empty-cell">No se encontraron productos.</p>
            </div>
          </div>

          <!-- COLUMNA DERECHA -->
          <div class="pedido-right premium-panel">
            <div class="panel-title">
              <h3>Resumen del pedido</h3>
            </div>

            <div class="cart-list premium-cart-list">
              <p v-if="cart.length === 0" class="empty-cell">No hay productos en el pedido.</p>

              <div class="cart-item premium-cart-item" v-for="item in cart" :key="item.id">
                <div class="cart-item-info">
                  <strong>{{ item.name }}</strong>
                  <small>{{ item.code }}</small>
                  <span>${{ item.price.toFixed(2) }} c/u</span>
                </div>

                <div class="cart-item-actions">
                  <button class="qty-btn" @click="changeQuantity(item.id, -1)">-</button>
                  <span class="qty-number">{{ item.quantity }}</span>
                  <button class="qty-btn" @click="changeQuantity(item.id, 1)">+</button>
                  <button class="remove-btn" @click="removeFromCart(item.id)">✕</button>
                </div>
              </div>
            </div>

            <div class="summary premium-summary">
              <p><span>Subtotal</span><span>${{ subtotal.toFixed(2) }}</span></p>
              <p><span>IVA 15%</span><span>${{ tax.toFixed(2) }}</span></p>
              <p class="total"><span>Total</span><span>${{ total.toFixed(2) }}</span></p>
            </div>

            <div class="actions vertical-actions premium-actions">
              <button @click="saveOrder">Guardar pedido</button>
              <button class="secondary" @click="clearOrder">Limpiar</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>
