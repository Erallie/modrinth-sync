# Modrinth Sync
A set of workflows that allow your plugin releases to be automatically pushed to modrinth.

These workflows sync your modrinth releases when you do the following on GitHub:
- Publish a new release
    - It uses your attached `.jar` binary as the primary file.
    - It gets the version from your GitHub tag.
    - It gets the release name from your GitHub release name.
- Edit a release
    - Can edit all synced fields except the primary file.
- Delete a release
    - It deletes the release from your modrinth project
- Edit your `README.md`
    - It uses your `README.md` as your project description.

# Setup
1. Create a file named `modrinth-sync.yml` in the folder `.github/workflows` in your GitHub repository.
2. Copy and paste the following into the file:
    ```yml
    name: Modrinth Sync

    on:
        release:
            types: [published, edited, deleted]
        push:
            branches:
                - main

    jobs:
        upload-release:
            if: github.event_name == 'release' && github.event.action == 'published'
            uses: Erallie/modrinth-sync/.github/workflows/publish-release.yml@main
            with:
                project_id: "XXXXXXXX"
                game_versions: '["1.21", "1.21.1", "1.21.2", "1.21.3", "1.21.4"]'
                loaders: '["spigot", "paper", "purpur"]'
                dependencies: >-
                    [
                        { "version": "*", "project_id": "XXXXXXXX", "dependency_type": "required" },
                        { "version": "*", "project_id": "XXXXXXXX", "dependency_type": "required" }
                    ]
            secrets:
                MODRINTH_TOKEN: ${{ secrets.MODRINTH_TOKEN }}
        
        edit-release:
            if: github.event_name == 'release' && github.event.action == 'edited'
            uses: Erallie/modrinth-sync/.github/workflows/edit-release.yml@main
            with:
                project_id: "XXXXXXXX"
            secrets:
                MODRINTH_TOKEN: ${{ secrets.MODRINTH_TOKEN }}
        
        delete-release:
            if: github.event_name == 'release' && github.event.action == 'deleted'
            uses: Erallie/modrinth-sync/.github/workflows/delete-release.yml@main
            with:
                project_id: "XXXXXXXX"
            secrets:
                MODRINTH_TOKEN: ${{ secrets.MODRINTH_TOKEN }}

        edit-project-description:
            if: github.event_name == 'push'
            uses: Erallie/modrinth-sync/.github/workflows/edit-project-description.yml@main
            with:
                project_id: "XXXXXXXX"
            secrets:
                MODRINTH_TOKEN: ${{ secrets.MODRINTH_TOKEN }}
    ```
3. Replace the `project_id` in all actions with your Modrinth project's id.
4. Replace `game_verions`, `loaders`, and `dependencies` under `upload-release` with the correct data for your Modrinth project.
5. Commit the changes.