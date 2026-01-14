# 🚔 ER:LC API Wrapper (Fork)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

#### ⚠️ Note about this Fork: This version has been modified to work without a Global API key by using serverTokens directly. The initialization of the Client class is now optional, allowing the use of standalone functions. This fork also adds missing TypeScript types and includes direct links to the official API documentation for every function.

A comprehensive, lightweight, and fully-typed API wrapper for Emergency Response: Liberty County (ER:LC) with 100% API coverage, robust error handling, and TypeScript support.

## ✨ Features

- 🎯 **100% API Coverage** - All ER:LC API endpoints supported
- 🛡️ **Robust Error Handling** - Comprehensive error catching and meaningful error messages
- 📝 **Full TypeScript Support** - Complete type definitions for all methods and responses
- ⚡ **Optimized Performance** - Efficient request handling with timeout management
- 🔒 **Secure** - Built-in validation and secure token handling
- 📚 **Well Documented** - Extensive documentation and examples
- 🚀 **Easy to Use** - Simple, intuitive API design

## 📦 Installation

```bash
npm install github:EldamianO7/ERLC-API
```

## 🚀 Setup Global API Key (Optional)

```typescript
import erlc from "erlc-api";

const client = new erlc.Client({
  globalToken: "your-global-token-here",
});

client.config();
```

## 📖 API Methods

##### Note: Examples are shown in TypeScript, but this library is fully compatible with JavaScript.

### 🖥️ Server Information

#### Get Server Details

```typescript
import { getServer } from "erlc-api";

const getServerInfo = async () => {
  try {
    const serverToken = "your-server-api-key"; // From Server Settings
    const server = await getServer(serverToken);

    console.log(server);
    /*
    Expected Response:
    {
      Name: "Your Server Name",
      OwnerUsername: "ServerOwner",
      CoOwnerUsernames: ["CoOwner1", "CoOwner2"],
      CurrentPlayers: 25,
      MaxPlayers: 40,
      JoinKey: "ABC123",
      AccVerifiedReq: "Disabled", // "Email" | "Phone/ID"
      TeamBalance: true,
      VanityURL: "https://policeroleplay.community/join?code=ABC123"
    }
    */
  } catch (error) {
    console.error("Error fetching server info:", error.message);
  }
};
```

#### Get Current Players

```typescript
import { getPlayers } from "erlc-api";

const getCurrentPlayers = async () => {
  try {
    const players = await getPlayers(serverToken);

    console.log(players);
    /*
    Expected Response:
    [
      {
        Player: "PlayerName:123456789",
        Permission: "Server Owner", // "Member" | "Moderator" | "Server Administrator"
        Team: "Police" // "Civilian" | "Fire" | "Sheriff"
      }
    ]
    */
  } catch (error) {
    console.error("Error fetching players:", error.message);
  }
};
```

#### Get Server Queue

```typescript
import { getQueue } from "erlc-api";

const getServerQueue = async () => {
  try {
    const queue = await getQueue(serverToken);
    console.log(`Players in queue: ${queue.length}`);
  } catch (error) {
    console.error("Error fetching queue:", error.message);
  }
};
```

### 👥 Staff Management

#### Get Staff Information (DEPRECATED)

This endpoint is deprecated. Although the developers have stated it will not be altered for now, use it with caution as it is no longer maintained.

```typescript
import { getStaff } from "erlc-api";

const getStaffInfo = async () => {
  try {
    const staff = await getStaff(serverToken);

    console.log(staff);
    /*
    Expected Response:
    {
      CoOwners: [123456789, 987654321],
      Admins: { "123456789": "AdminName" },
      Mods: { "987654321": "ModName" }
    }
    */
  } catch (error) {
    console.error("Error fetching staff:", error.message);
  }
};
```

### 📊 Server Logs

#### Get Join/Leave Logs

```typescript
import { getJoinLogs } from "erlc-api";

const getGameJoinLogs = async () => {
  try {
    const logs = await getJoinLogs(serverToken);

    logs.forEach((log) => {
      const action = log.Join ? "joined" : "left";
      console.log(
        `${log.Player} ${action} at ${new Date(log.Timestamp * 1000)}`
      );
    });
  } catch (error) {
    console.error("Error fetching join logs:", error.message);
  }
};
```

#### Get Kill Logs

```typescript
import { getKillLogs } from "erlc-api";

const getGameKillLogs = async () => {
  try {
    const kills = await getKillLogs(serverToken);

    kills.forEach((kill) => {
      console.log(
        `${kill.Killer} killed ${kill.Killed} at ${new Date(
          kill.Timestamp * 1000
        )}`
      );
    });
  } catch (error) {
    console.error("Error fetching kill logs:", error.message);
  }
};
```

#### Get Command Logs

```typescript
import { getCommandLogs } from "erlc-api";

const getGameCommandLogs = async () => {
  try {
    const commands = await getCommandLogs(serverToken);

    commands.forEach((cmd) => {
      console.log(`${cmd.Player} executed: ${cmd.Command}`);
    });
  } catch (error) {
    console.error("Error fetching command logs:", error.message);
  }
};
```

#### Get Moderator Call Logs

```typescript
import { getModcallLogs } from "erlc-api";

const getModCalls = async () => {
  try {
    const modcalls = await getModcallLogs(serverToken);

    modcalls.forEach((call) => {
      const status = call.Moderator
        ? `answered by ${call.Moderator}`
        : "unanswered";
      console.log(`${call.Caller} made a modcall - ${status}`);
    });
  } catch (error) {
    console.error("Error fetching modcall logs:", error.message);
  }
};
```

### 🚗 Vehicle Management

#### Get Server Vehicles

```typescript
import { getVehicles } from "erlc-api";

const getGameVehicles = async () => {
  try {
    const vehicles = await getVehicles(serverToken);

    vehicles.forEach((vehicle) => {
      console.log(
        `${vehicle.Name} owned by ${vehicle.Owner} - Texture: ${
          vehicle.Texture || "Default"
        }`
      );
    });
  } catch (error) {
    console.error("Error fetching vehicles:", error.message);
  }
};
```

### 🔨 Server Management

#### Execute Server Commands

```typescript
import { runCommand } from "erlc-api";

const executeCommand = async () => {
  try {
    const success = await runCommand(serverToken, ":h Welcome to our server!");

    if (success) {
      console.log("Command executed successfully!");
    }
  } catch (error) {
    console.error("Error executing command:", error.message);
  }
};
```

#### Get Server Bans

```typescript
import { getBans } from "erlc-api";

const getBannedPlayers = async () => {
  try {
    const bans = await getBans(serverToken);

    Object.entries(bans).forEach(([playerId, playerName]) => {
      console.log(`${playerName} (${playerId}) is banned`);
    });
  } catch (error) {
    console.error("Error fetching bans:", error.message);
  }
};
```

## 🛠️ Advanced Usage

### Error Handling Best Practices

```typescript
import { getServer } from "erlc-api";

const handleApiCall = async () => {
  try {
    const result = await getServer(serverToken);
    return result;
  } catch (error) {
    // The error is now an ErlcError with detailed information
    console.error(`Error ${error.code}: ${error.message}`);
    console.error(`Category: ${error.category}, Severity: ${error.severity}`);

    // Handle specific ERLC error codes
    switch (error.code) {
      case 2002:
        console.error(
          "Invalid server key - get a new one from server settings"
        );
        break;
      case 4001:
        console.error("Rate limited - reduce request frequency");
        break;
      case 3002:
        console.error("Server offline - wait for players to join");
        break;
      case 9999:
        console.error("Server module outdated - restart server");
        break;
    }

    // Show suggested actions
    if (error.suggestions) {
      console.error("Suggested actions:");
      error.suggestions.forEach((action) => console.error(`- ${action}`));
    }

    // Check if error is retryable
    if (error.retryable) {
      console.error("This error might be resolved by retrying");
    }

    throw error; // Re-throw if needed
  }
};
```

### Batch Operations

```typescript
import { getServer, getPlayers, getStaff, getVehicles } from "erlc-api";

const getServerOverview = async (serverToken) => {
  try {
    // Execute multiple API calls concurrently
    const [serverInfo, players, staff, vehicles] = await Promise.all([
      getServer(serverToken),
      getPlayers(serverToken),
      getStaff(serverToken),
      getVehicles(serverToken),
    ]);

    return {
      server: serverInfo,
      playerCount: players.length,
      staffCount:
        Object.keys(staff.Admins).length + Object.keys(staff.Mods).length,
      vehicleCount: vehicles.length,
    };
  } catch (error) {
    console.error("Error getting server overview:", error.message);
    throw error;
  }
};
```

## 🔑 Authentication

**Server Token**: Found in your server settings within ER:LC

## 📝 TypeScript Support

The package includes comprehensive TypeScript definitions:

```typescript
import {
  getServer,
  getPlayers,
  getJoinLogs,
  type ServerStatus,
  type ServerPlayer,
  type JoinLog,
} from "erlc-api";

// Fully typed responses
const server: ServerStatus = await getServer(serverToken);
const players: ServerPlayer[] = await getPlayers(serverToken);
const joinLogs: JoinLog[] = await getJoinLogs(serverToken);
```

## ⚡ Performance Tips

1. **Use Promise.all()** for concurrent requests when fetching multiple endpoints.
2. **Implement caching** for frequently accessed data that doesn't change often (like server info or staff lists).
3. **Handle rate limits** by implementing retry logic with exponential backoff (see example below).
4. **Use timeouts** - all methods have built-in 10-15 second timeouts to prevent hanging requests.
5. **Respect the Rate Limits**: Standard Server Tokens are limited to approximately **2 credits per minute**. Avoid calling functions inside fast loops. Frequent limit violations may lead to temporary API bans or stricter restrictions!
6. **Adaptive Design**: Avoid hardcoding fixed cooldowns. Instead, rely on the error responses to adjust your request frequency dynamically, as API limits may change without notice.

## 🐛 Error Types

The wrapper provides comprehensive error handling with specific ERLC error codes:

### ERLC Error Codes

| Code | Category             | Description                            |
| ---- | -------------------- | -------------------------------------- |
| 0    | System Error         | Unknown error occurred                 |
| 1001 | Communication Error  | Error communicating with Roblox server |
| 1002 | System Error         | Internal system error                  |
| 2000 | Authentication Error | Missing server key                     |
| 2001 | Authentication Error | Invalid server key format              |
| 2002 | Authentication Error | Invalid or expired server key          |
| 2003 | Authentication Error | Invalid global API key                 |
| 2004 | Authentication Error | Server key banned                      |
| 3001 | Request Error        | Invalid command provided               |
| 3002 | Request Error        | Server offline (no players)            |
| 4001 | Rate Limit Error     | Rate limited                           |
| 4002 | Permission Error     | Restricted command                     |
| 4003 | Content Error        | Prohibited message                     |
| 9998 | Access Error         | Restricted resource                    |
| 9999 | Version Error        | Outdated server module                 |

### Error Properties

All errors are instances of `ErlcError` with these properties:

- `code`: ERLC error code or HTTP status
- `message`: Human-readable error message
- `category`: Error category (e.g., "AUTHENTICATION_ERROR")
- `severity`: Error severity ("LOW", "MEDIUM", "HIGH", "CRITICAL")
- `suggestions`: Array of suggested actions to resolve the error
- `retryable`: Boolean indicating if the error might be resolved by retrying
- `timestamp`: ISO timestamp when the error occurred

### Retry Logic Example

```typescript
import { getPlayers } from "erlc-api";

async function withRetry(apiCall, maxRetries = 3) {
  for (let attempt = 1; attempt <= maxRetries; attempt++) {
    try {
      return await apiCall();
    } catch (error) {
      if (!error.retryable || attempt === maxRetries) {
        throw error;
      }

      // Wait before retrying with exponential backoff
      const delay = 1000 * Math.pow(2, attempt - 1);
      await new Promise((resolve) => setTimeout(resolve, delay));
    }
  }
}

// Usage
const players = await withRetry(() => getPlayers(serverToken));
```

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🔗 Links

- **Original ER:LC API Wrapper**: [ER:LC API Wrapper](https://github.com/Exodo0/ERLC-API/)
- **API Documentation**: [PRC API Docs](https://apidocs.policeroleplay.community/reference/api-reference)
- **Discord Support**: [Join PRC Discord](https://discord.gg/prc)

## 👨‍💻 Credits

- **Library Development**: [Egologics](https://twitter.com/0Adexus0)
- **API Development**: [Police Roleplay Community](https://twitter.com/PRC_Roblox)
- **Community Support**: [PRC Discord Community](https://discord.gg/prc)
