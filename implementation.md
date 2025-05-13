# Implementation Plan: d365-agent-mcpserver-ts

This document outlines the phased implementation plan for the `d365-agent-mcpserver-ts` repository. This service is a TypeScript/Node.js implementation of a Model Context Protocol (MCP) Server.

## Overall Goal
To build a functional MCP server in TypeScript that can expose various tools. While its use for direct Dynamics 365 integration is currently secondary to `d365-agent-mcpserver-dotnet` (due to issues with the TypeScript D365 OData client), this server can be used for other purposes or for D365 if the OData client situation improves.

---

## Phase 1: Core MCP Server Setup & Basic Non-D365 Tooling

*   **Objectives:**
    *   Establish the Node.js/TypeScript project structure for the MCP server.
    *   Integrate the TypeScript MCP Server SDK.
    *   Implement basic MCP handshake (`initialize`, `discoverTools`).
    *   Expose 1-2 simple non-D365 specific tools (e.g., a calculator, a string utility tool).
    *   Containerize and set up CI/CD.
*   **Tasks:**
    1.  **Project Initialization:**
        *   Create a new Node.js/TypeScript project (e.g., using Express, Fastify, or a bare Node HTTP server if the MCP SDK handles routing).
        *   Set up linting, formatting, build scripts (e.g., using `tsc` or `tsup`), and testing framework (e.g., Jest, Vitest).
    2.  **MCP Server SDK Integration:**
        *   Add dependency on the TypeScript MCP Server SDK (e.g., `@modelcontext/server` from `submodules/typescript-sdk/packages/mcp-server` or its npm equivalent).
        *   Implement the server startup logic, including MCP `initialize` handshake handler and `discoverTools` handler.
    3.  **Implement Basic Non-D365 Tools:**
        *   Define and implement 1-2 simple tools that do not require D365 access.
            *   Example: `calculatorTool` (input: operation, numbers; output: result).
            *   Example: `stringFormatterTool` (input: template string, values; output: formatted string).
        *   Define input/output schemas for these tools (e.g., using Zod, as supported by some MCP SDKs).
    4.  **Error Handling:**
        *   Implement basic error handling within tools, returning structured `McpResponse` errors.
    5.  **Containerization:**
        *   Create a Dockerfile for the `d365-agent-mcpserver-ts` application.
    6.  **CI/CD Pipeline:**
        *   Set up a CI/CD pipeline (e.g., GitHub Actions) to build the Docker image, publish it to Azure Container Registry, and deploy to Azure Container Apps.
*   **Testing:**
    *   Unit tests for the implemented tools.
    *   Integration tests to verify MCP `initialize`, `discoverTools`, and `executeTool` calls using `d365-agent-mcpclient-ts`.
    *   Test deployment to a container environment.
*   **Deliverables:**
    *   A functional TypeScript MCP Server capable of exposing basic, non-D365 tools.
    *   Container image and deployment pipeline.

---

## Phase 2: D365 Integration Attempt & Advanced Tooling (Conditional)

*   **Objectives:**
    *   Attempt to integrate D365 tools if the `d365-agent-odataclient-ts` issues are resolved or workarounds are found.
    *   Explore exposing tools for other backend systems or custom logic.
*   **Tasks:**
    1.  **Re-evaluate `d365-agent-odataclient-ts`:**
        *   If the TypeScript OData client for D365 (from `d365-agent-odataclient-ts`) becomes stable and functional:
            *   Add it as a dependency.
            *   Implement D365 authentication logic (e.g., MSAL for Node.js, handling secrets via Key Vault).
            *   Implement a selection of D365 MCP tools (e.g., `getCustomerByNameTS`, `createSalesOrderTS`) using this OData client.
            *   Conduct thorough testing against a D365 sandbox.
    2.  **Tools for Other Systems/Logic:**
        *   Identify needs for tools that interact with other APIs, databases, or perform custom computations.
        *   Implement these tools within this server, defining their schemas and logic.
    3.  **Advanced MCP Features:**
        *   Explore and implement support for advanced MCP features like tool progress updates or streaming outputs if required by consuming agents.
*   **Testing:**
    *   Rigorous integration tests for any D365 tools if implemented.
    *   Unit and integration tests for any new non-D365 tools.

---

## Phase 3 & 4: Long-term Maintenance & Specialization

*   **Objectives:**
    *   Maintain the server and adapt it to evolving needs.
    *   Potentially specialize this server for non-D365 tools if the .NET MCP server remains the primary for D365.
*   **Tasks:**
    1.  **Ongoing Maintenance:**
        *   Address bugs, update dependencies, and ensure compatibility with MCP clients.
    2.  **Performance & Scalability:**
        *   Optimize performance as needed, especially if handling many requests or complex tools.
    3.  **Security:**
        *   Regularly review and update security configurations, authentication mechanisms, and input validation for tools.
    4.  **Documentation:**
        *   Maintain clear documentation for all exposed tools, their schemas, and any specific setup or usage instructions for this server.
*   **Testing:**
    *   Ongoing regression testing.
    *   Performance and security testing as appropriate.

This plan allows for the `d365-agent-mcpserver-ts` to be developed as a general-purpose TypeScript MCP server, with a conditional path for D365 integration depending on the readiness of the TypeScript OData client.
