<template>
    <div class="mb-2">
    <b-navbar toggleable="lg" type="dark" variant="dark">
        <b-navbar-brand href="/">AppDemo</b-navbar-brand>

        <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

        <b-collapse id="nav-collapse" is-nav>
        <b-navbar-nav>
            <b-nav-item to="/">Home</b-nav-item>
            <b-nav-item to="/department">Departments</b-nav-item>
            <b-nav-item to="/employee">Employees</b-nav-item>
            <b-nav-item to="/group">Groups</b-nav-item>
            <b-nav-item to="/project">Projects</b-nav-item>
        </b-navbar-nav>

        </b-collapse>
    </b-navbar>
    </div>
</template>