<script setup>
import { onMounted, ref } from 'vue';
import { useRouter } from 'vue-router';
import { logout, subscribeToAuthState } from '../services/auth';

// Obtenemos el router a través del composable useRouter.
const router = useRouter();

// Datos del usuario autenticado.
const user = ref({
    id: null,
    email: null,
});

onMounted(() => {
    // Nos suscribimos al estado de autenticación.
    subscribeToAuthState(newUserData => user.value = newUserData);
});

function handleLogout() {
    logout();

    // Redireccionamos al login.
    router.push('/ingresar');
}
</script>

<template>
    <nav class="flex items-center gap-8 p-4 bg-slate-200">
        <RouterLink class="text-xl" to="/">DV Social</RouterLink>

        <ul class="flex gap-4">
            <li>
                <RouterLink to="/">Feed</RouterLink>
            </li>
            <template v-if="user.id === null">
                <li>
                    <RouterLink to="/ingresar">Ingresar</RouterLink>
                </li>
                <li>
                    <RouterLink to="/crear-cuenta">Crear cuenta</RouterLink>
                </li>
            </template>
            <template v-else>
                <li>
                    <RouterLink to="/crear-publicacion">Crear publicación</RouterLink>
                </li>
                <li>
                    <RouterLink to="/chat">Chat</RouterLink>
                </li>
                <li>
                    <RouterLink to="/mi-perfil">Mi perfil</RouterLink>
                </li>
                <li>
                    <form 
                        action="#"
                        @submit.prevent="handleLogout"
                    >
                        <button type="submit">{{ user.email }} (Cerrar sesión)</button>
                    </form>
                </li>
            </template>
        </ul>
    </nav>
</template>