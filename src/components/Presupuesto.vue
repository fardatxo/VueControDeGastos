<template>
<form class="presupuesto">
    <div class="campo">
        <label for="nuevo-presupuesto">Definir Presupuesto</label>
        <input id="nuevo-presupuesto" class="nuevo-presupuesto" placeholder="Indica tu presupuesto" type="number"/>      
    </div>
    <input type="submit" value="Aceptar" />
</form>
</template>

<style scoped>
.presupuesto {
    width: 100%;
}

.campo {
    display: grid;
    gap: 2rem;
}

.presupuesto label {
    font-size: 2.2rem;
    text-align: center;
    color: var(--azul);
}

.presupuesto input[type="number"] {
    background-color: var(--gris-claro);
    border-radius: 1rem;
    padding: 1rem;
    border: none;
    font-size: 2.2rem;
    text-align: center;
}

.presupuesto input[type="submit"] {
    background-color: var(--azul);
    border: none;
    padding: 1rem;
    font-size: 2rem;
    text-align: center;
    margin-top: 2rem;
    color: var(--blanco);
    font-weight: 900;
    width: 100%;
    transition: background-color 300ms ease;
}

.presupuesto input[type="submit"]:hover {
    background-color: #1048A4;
    cursor: pointer;
}

.contenedor {
    width: 90%;
    max-width: 80rem;
    margin: 0 auto;
}

.contenedor-header {
    margin-top: -5rem;
    transform: translateY(5rem);
    padding: 5rem;
}

.sombra {
    box-shadow: 0px 10px 15px -3px rgba(0, 0, 0, 0.1);
    background-color: var(--blanco);
    border-radius: 1.2rem;
    padding: 5rem;
}
</style>