<script>
    import '../app.scss'; // Bootstrap 5
    import supabase from '$lib/db';
    import { page, session } from '$app/stores';
    import { browser } from '$app/env';
    import { goto } from '$app/navigation';

    // code that runs only in the browser
    if (browser) {
   	 $session = supabase.auth.session(); // set session
   	 redirect();

   	 supabase.auth.onAuthStateChange((userSession) => {
   		 $session = userSession; // set session
   		 redirect();
   	 });
    }

    function redirect() {
   	 //login redirect
   	 if ($session && $page.path === '/') {
   		 goto('/welcome');
   	 }

   	 //logout redirect
   	 if (!$session && $page.path === '/welcome') {
   		 goto('/');
   	 }
    }
</script>

<slot />
