<template>
    <div class="container">
        <header class="header">
            <h1>Static IP Command Generator</h1>
        </header>
        <RouterLink to="/">Back to Home</RouterLink>

        <main>
            <form @submit.prevent="generateCommand" class="form">
                <div class="form-group">
                    <label for="connection-name">Connection Name:</label>
                    <input
                        v-model="connectionName"
                        id="connection-name"
                        type="text"
                        required
                        placeholder="e.g., Wired connection 1"
                    />
                </div>

                <div class="form-group" v-if="ipMethod === 'manual'">
                    <label for="host-ip">Host IP Address:</label>
                    <input
                        v-model="hostIp"
                        id="host-ip"
                        type="text"
                        placeholder="e.g., 192.168.1.100/24"
                        required
                    />
                </div>

                <div class="form-group" v-if="ipMethod === 'manual'">
                    <label for="gateway">IP Gateway:</label>
                    <input
                        v-model="gateway"
                        id="gateway"
                        type="text"
                        placeholder="e.g., 192.168.1.1"
                        required
                    />
                </div>

                <div class="form-group" v-if="ipMethod === 'manual'">
                    <label for="dns">DNS Servers:</label>
                    <input
                        v-model="dns"
                        id="dns"
                        type="text"
                        placeholder="e.g., 8.8.8.8,8.8.4.4"
                        required
                    />
                </div>

                <div class="form-group" v-if="ipMethod === 'manual'">
                    <label for="domain">Domain Name:</label>
                    <input v-model="domainName" id="domain" type="text" placeholder="Optional" />
                </div>

                <div class="form-group">
                    <label for="method">IP Method:</label>
                    <select v-model="ipMethod" id="method">
                        <option value="manual">Manual</option>
                        <option value="auto">DHCP</option>
                    </select>
                </div>

                <button type="submit" class="btn">Generate Command</button>
            </form>

            <div v-if="command" class="output">
                <h2>Generated Command:</h2>
                <code>{{ command }}</code>
            </div>
        </main>
    </div>
</template>

<script>
export default {
    data() {
        return {
            connectionName: 'Wired connection 1',
            hostIp: '',
            gateway: '',
            dns: '',
            domainName: '',
            ipMethod: 'manual',
            command: ''
        }
    },
    methods: {
        generateCommand() {
            if (this.ipMethod === 'manual') {
                this.command = `nmcli con mod "${this.connectionName}" \
  ipv4.addresses "${this.hostIp}" \
  ipv4.gateway "${this.gateway}" \
  ipv4.dns "${this.dns}" \
  ipv4.dns-search "${this.domainName}" \
  ipv4.method "${this.ipMethod}"`
            } else {
                this.command = `nmcli con mod "${this.connectionName}" \
  ipv4.addresses "" \
  ipv4.gateway "" \
  ipv4.dns "" \
  ipv4.dns-search "" \
  ipv4.method "${this.ipMethod}"`
            }
        }
    }
}
</script>

<style src="../assets/layout.css"></style>
<style src="../assets/forms.css"></style>
<style src="../assets/typography.css"></style>
