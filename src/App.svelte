<script>
    import ContactCard from './ContactCard.svelte';

    let name = 'Max';
    let title = '';
    let image = '';
    let description = '';
    let formState = 'empty';
    let createdContacts = [];

    function addCard(event) {
        // event.preventDefault(); This can be done using event Modifier |preventDefault in the on:click
        if (
            name.trim().length === 0 ||
            title.trim().length === 0 ||
            image.trim().length === 0 ||
            description.trim().length === 0
        ) {
            formState = 'invalid';
            return;
        }
        // This will not trigger an update
        // createdContacts.push({
        //     name: name,
        //     jobTitle: title,
        //     imageUrl: image,
        //     desc: description,
        // });
        // so we need to use the equal sign in order for the contact card to show
        createdContacts = [
            ...createdContacts, // this is to add the existing createdContacts
            {
                id: Math.random(),
                name: name,
                jobTitle: title,
                imageUrl: image,
                desc: description,
            },
        ];
        formState = 'valid';
    }

    function deleteFirst() {
        createdContacts = createdContacts.slice(1);
    }

    function deleteLast() {
        createdContacts = createdContacts.slice(0, -1);
    }
</script>

<form>
    <div id="form">
        <div class="form-control">
            <label for="userName">User Name</label>
            <input type="text" bind:value={name} id="userName" />
        </div>
        <div class="form-control">
            <label for="jobTitle">Job Title</label>
            <input type="text" bind:value={title} id="jobTitle" />
        </div>
        <div class="form-control">
            <label for="image">Image URL</label>
            <input type="text" bind:value={image} id="image" />
        </div>
        <div class="form-control">
            <label for="desc">Description</label>
            <textarea rows="3" bind:value={description} id="desc" />
        </div>
    </div>
    <button on:click|preventDefault={addCard}>Add Card</button>
</form>

<button on:click={() => (createdContacts = createdContacts.slice(1))}>Delete First</button>
<button on:click={deleteLast}>Delete Last</button>

{#if formState === 'invalid'}
    <p>Invalid input.</p>
{:else}
    <p>Please enter some data and Hit the button!</p>
{/if}

{#each createdContacts as contact, index (contact.id)}
    <p># {index + 1}</p>
    <ContactCard
        userName={contact.name}
        jobTitle={contact.jobTitle}
        description={contact.desc}
        userImage={contact.imageUrl}
    />
{:else}
    <p>Please add some content, we found none.</p>
{/each}

<style>
    #form {
        width: 30rem;
        max-width: 100%;
    }
</style>
