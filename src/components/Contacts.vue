<template>
  <div class="contacts">
    <h1>Contacts <a v-if="action == 'list'" class="btn btn-primary" @click.prevent="changeAction('new')">Add New</a><a v-else class="btn btn-primary" @click.prevent="changeAction('list')">Show All</a></h1>

    <form v-if="action == 'new' || action == 'edit'" style="text-align: left;">
      <div class="form-group">
        <label>Name</label>
        <input type="name" class="form-control" v-model="name">
      </div>
      <div class="form-group">
        <label>Email</label>
        <input type="email" class="form-control" v-model="email">
      </div>
      <div class="form-group">
        <label>Phone</label>
        <input type="text" class="form-control" v-model="phone">
      </div>
      <div class="form-actions">
        <button v-if="action == 'edit'" type="submit" class="btn btn-form btn-primary" @click.prevent="updateContact(currentContactId)">Update</button>
        <button v-else type="submit" class="btn btn-form btn-primary" @click.prevent="saveContact">Save</button>
      </div>
    </form>

    <table class="table-striped" v-if="action == 'list'">
      <thead>
        <tr>
          <th>Name</th>
          <th>Email</th>
          <th>Phone</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(contact, index) in contacts" :key="index">
          <td>{{ contact.name }}</td>
          <td>{{ contact.email }}</td>
          <td>{{ contact.phone }}</td>
          <td><a href="#" @click.prevent="editContact(contact)"><span class="icon icon-pencil"></span></a>  <a href="#" @click.prevent="removeContact(contact)"><span class="icon icon-cancel-circled"></span></a></td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      action: "list",
      contacts:
        typeof Storage != undefined && localStorage.contacts
          ? JSON.parse(localStorage.getItem("contacts"))
          : [],
      currentContactId: null,
      name: "",
      email: "",
      phone: ""
    };
  },
  methods: {
    changeAction(action) {
      this.action = action;
      (this.name = ""), (this.email = ""), (this.phone = "");
    },
    saveContact() {
      this.action = "new";

      let newContact = {
        name: this.name,
        email: this.email,
        phone: this.phone
      };
      this.contacts.push(newContact);

      // update locale storage
      if (Storage != undefined) {
        localStorage.contacts = JSON.stringify(this.contacts);
      }

      this.action = "list";
    },
    editContact(contact) {
      this.action = "edit";
      this.currentContactId = this.contacts.indexOf(contact);

      this.name = contact.name;
      this.email = contact.email;
      this.phone = contact.phone;
    },
    removeContact(contact) {
      let index = this.contacts.indexOf(contact);
      this.contacts.splice(index, 1);

      // update locale storage
      if (Storage != undefined) {
        localStorage.contacts = JSON.stringify(this.contacts);
      }
    },
    updateContact(id) {
      this.contacts[id].name = this.name;
      this.contacts[id].email = this.email;
      this.contacts[id].phone = this.phone;

      // update locale storage
      if (Storage != undefined) {
        localStorage.contacts = JSON.stringify(this.contacts);
      }

      this.action = "list";
    }
  }
};
</script>

<style scoped>
.contacts {
  text-align: center;
  margin: 0 auto;
}
</style>
