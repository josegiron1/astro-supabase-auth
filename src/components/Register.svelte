<script lang="ts">
    import { supabase } from "../supabase";

    let email = "";
    let password = "";

    const handleSubmit = async () => {
        console.log("Submitted");
        console.log(email, password);
        const { data, error } = await supabase.auth.signUp({
            email,
            password,
        });
        if (error) {
            alert(error.message);
            return;
        }
        const loginData = await fetch("/api/login", {
            method: "POST",
            headers: new Headers({ "Content-Type": "application/json" }),
            credentials: "same-origin",
            body: JSON.stringify({
                access_token: data.session?.access_token,
                expires_in: data.session?.expires_in,
                refresh_token: data.session?.refresh_token,
            }),
        });
        if (loginData.ok) {
            window.location.replace("/");
        }
    };
</script>

<p class="text-3xl">Sign Up</p>
<form class="flex flex-col gap-3" on:submit|preventDefault={handleSubmit}>
    <div>
        <label for="email" class="label">
            <span class="label-text">Email</span>
        </label>
        <input
            id="email"
            type="email"
            placeholder="Email"
            class="input input-bordered w-full max-w-xs"
            bind:value={email}
        />
    </div>
    <div>
        <label class="label" for="password">
            <span class="label-text">Password</span>
        </label>
        <input
            id="password"
            autocomplete="off"
            type="password"
            placeholder="Password"
            class="input input-bordered w-full max-w-xs"
            bind:value={password}
        />
    </div>
    <button class="btn">Sign Up</button>
</form>
