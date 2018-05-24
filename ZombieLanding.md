-----------------------------------------------------------------------------------------
-- Created by: Aayman Shameem
-- Created on: May 24, 2018
-- 
-- This code will take the user to splash scene
-----------------------------------------------------------------------------------------

-- STARTING PLACE

local composer = require( "composer" )

display.setStatusBar( display.HiddenStatusBar )

composer.gotoScene( "scene.splashScene" )

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

-----------------------------------------------------------------------------------------
-- Created by: Aayman Shameem
-- Created on: May 24, 2018
-- 
-- This code will take the user to the menu scene
-----------------------------------------------------------------------------------------

-- SPLASH SCENE

local composer = require( "composer" )
 
local scene = composer.newScene()
 
-- -----------------------------------------------------------------------------------
-- Code outside of the scene event functions below will only be executed ONCE unless
-- the scene is removed entirely (not recycled) via "composer.removeScene()"
-- -----------------------------------------------------------------------------------
local function showMenuScene()
    local options = {
        effect = "fade",
        time = 500
    }
    composer.gotoScene( "scene.menuScene" , options )
end 
 
 
 
-- -----------------------------------------------------------------------------------
-- Scene event functions
-- -----------------------------------------------------------------------------------
 
-- create()
function scene:create( event )
 
    local sceneGroup = self.view
 
end
 
 
-- show()
function scene:show( event )
 
    local sceneGroup = self.view
    local phase = event.phase
 
    if ( phase == "will" ) then
        local background = display.newRect(display.contentCenterX, display.contentCenterY, display.contentWidth, display.contentHeight)
        background:setFillColor( 0,1,1 )
        sceneGroup:insert( background )

        local title = display.newText("Splash Scene", display.contentWidth / 2, display.contentHeight / 2, native.systemFont, 48)
        title:setFillColor( 0,0,0 )
        sceneGroup:insert( title )
    
 
    elseif ( phase == "did" ) then
        timer.performWithDelay( 2000, showMenuScene, 1)
 
    end
end
 
 
-- hide()
function scene:hide( event )
 
    local sceneGroup = self.view
    local phase = event.phase
 
    if ( phase == "will" ) then
        -- Code here runs when the scene is on screen (but is about to go off screen)
 
    elseif ( phase == "did" ) then
        -- Code here runs immediately after the scene goes entirely off screen
 
    end
end
 
 
-- destroy()
function scene:destroy( event )
 
    local sceneGroup = self.view
    -- Code here runs prior to the removal of scene's view
 
end
 
 
-- -----------------------------------------------------------------------------------
-- Scene event function listeners
-- -----------------------------------------------------------------------------------
scene:addEventListener( "create", scene )
scene:addEventListener( "show", scene )
scene:addEventListener( "hide", scene )
scene:addEventListener( "destroy", scene )
-- -----------------------------------------------------------------------------------
 
return scene

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

-----------------------------------------------------------------------------------------
-- Created by: Aayman Shameem
-- Created on: May 24, 2018
-- 
-- This code will take the user to the game scene
-----------------------------------------------------------------------------------------

-- MENU SCENE

local composer = require( "composer" )
local widget = require("widget")
 
local scene = composer.newScene()
 
-- -----------------------------------------------------------------------------------
-- Code outside of the scene event functions below will only be executed ONCE unless
-- the scene is removed entirely (not recycled) via "composer.removeScene()"
-- -----------------------------------------------------------------------------------
local function showGameScene()
    local options = {
        effect = "fade",
        time = 500
    }
    composer.gotoScene( "scene.gameScene" , options )
end 
 
 
 
-- -----------------------------------------------------------------------------------
-- Scene event functions
-- -----------------------------------------------------------------------------------
 
-- create()
function scene:create( event )
 
    local sceneGroup = self.view
    -- Code here runs when the scene is first created but has not yet appeared on screen
 
end
 
 
-- show()
function scene:show( event )
 
    local sceneGroup = self.view
    local phase = event.phase
 
    if ( phase == "will" ) then
        local background = display.newRect(display.contentCenterX, display.contentCenterY, display.contentWidth, display.contentHeight)
        background:setFillColor( 1,1,0 )
        sceneGroup:insert( background )

        local title = display.newText("Menu Scene", display.contentWidth / 2, display.contentHeight / 2, native.systemFont, 48)
        title:setFillColor( 0,0,0 )
        sceneGroup:insert( title )


        local button = widget.newButton
        {
            label = "Go to Game",
            fontSize = 48,
            -- Properties for a rounded rectangle button
            shape = "roundedRect",
            width = 320,
            height = 100,
            cornerRadius = 2,
            fillColor = { default={1,1,1,1}, over={1,0.1,0.7,0.4} },
            strokeColor = { default={1,0.4,0,1}, over={0.8,0.8,1,1} },
            strokeWidth = 4,
            onEvent = function (event)
                if event.phase == "ended" then
                    composer.gotoScene("scene.gameScene")
                end
            end
        }
        button.x = display.contentWidth / 2
        button.y = display.contentHeight / 2 - 100
        sceneGroup:insert(button)
 
    elseif ( phase == "did" ) then
        -- Code here runs when the scene is entirely on screen
 
    end
end
 
 
-- hide()
function scene:hide( event )
 
    local sceneGroup = self.view
    local phase = event.phase
 
    if ( phase == "will" ) then
        -- Code here runs when the scene is on screen (but is about to go off screen)
 
    elseif ( phase == "did" ) then
        -- Code here runs immediately after the scene goes entirely off screen
 
    end
end
 
 
-- destroy()
function scene:destroy( event )
 
    local sceneGroup = self.view
    -- Code here runs prior to the removal of scene's view
 
end
 
 
-- -----------------------------------------------------------------------------------
-- Scene event function listeners
-- -----------------------------------------------------------------------------------
scene:addEventListener( "create", scene )
scene:addEventListener( "show", scene )
scene:addEventListener( "hide", scene )
scene:addEventListener( "destroy", scene )
-- -----------------------------------------------------------------------------------
 
return scene

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

-----------------------------------------------------------------------------------------
-- Created by: Aayman Shameem
-- Created on: May 24, 2018
-- 
-- This code will show the user a Zombie landing from the sky
-----------------------------------------------------------------------------------------

-- GAME SCENE

-- place all the require statements here
local composer = require( "composer" )
local physics = require("physics")
local json = require( "json" )
local tiled = require( "com.ponywolf.ponytiled" )
 
local scene = composer.newScene()
 
-- -----------------------------------------------------------------------------------
-- Code outside of the scene event functions below will only be executed ONCE unless
-- the scene is removed entirely (not recycled) via "composer.removeScene()"
-- -----------------------------------------------------------------------------------
 
 
 
 
-- -----------------------------------------------------------------------------------
-- Scene event functions
-- -----------------------------------------------------------------------------------
 
-- create()
function scene:create( event )
 
    local sceneGroup = self.view
    -- start physics
    physics.start()
    physics.setGravity(0, 32)
    physics.setDrawMode("normal")


    local filename = "assets/maps/level13.json"
    local mapData = json.decodeFile( system.pathForFile( filename, system.ResourceDirectory ) ) 
    map = tiled.new( mapData, "assets/maps" )


    --our character
    local sheetOptionsIdle = require( "assets.spritesheets.zombieMale.zombieMaleIdle" )
    local sheetIdleZombie = graphics.newImageSheet( "./assets/spritesheets/zombieMale/zombieMaleIdle.png", sheetOptionsIdle:getSheet() )

    local sheetOptionsWalk = require( "assets.spritesheets.zombieMale.zombieMaleWalk" )
    local sheetWalkingZombie = graphics.newImageSheet( "./assets/spritesheets/zombieMale/zombieMaleWalk.png", sheetOptionsWalk:getSheet() )

    --sequences table
    local sequence_data = {
    -- consecutive frames sequence
        {

            name = "idle",
            start = 1,
            count = 10,
            time = 800,
            loopCount = 0,
            sheet = sheetIdleZombie
        },
        {

            name = "walk",
            start = 1,
            count = 10,
            time = 1000,
            loopCount = 1,
            sheet = sheetWalkingZombie
        }
    }

    local zombie = display.newSprite( sheetIdleZombie, sequence_data )
    -- Add physics
    physics.addBody( zombie, "dynamic", { density = 3, bounce = 0, friction = 1.0 } )
    zombie.isFixedRotation = true
    zombie.x = display.contentWidth * .5
    zombie.y = 0
    zombie:setSequence( "idle" )
    zombie:play()

    sceneGroup:insert( map )
    sceneGroup:insert( zombie )



 
end
 
 
-- show()
function scene:show( event )
 
    local sceneGroup = self.view
    local phase = event.phase
 
    if ( phase == "will" ) then
        
        --[[local theMushroom = display.newImage( "./assets/sprites/environment/mushroom_red.png" )
        theMushroom.x = display.contentWidth / 2
        theMushroom.y = 0
        theMushroom.id = "the mushroom"
        physics.addBody( theMushroom, "dynamic", { 
            friction = 0.5, 
            bounce = 0.3 
            } )
        sceneGroup:insert( theMushroom )]]


 
    elseif ( phase == "did" ) then
        -- Code here runs when the scene is entirely on screen
 
    end
end
 
 
-- hide()
function scene:hide( event )
 
    local sceneGroup = self.view
    local phase = event.phase
 
    if ( phase == "will" ) then
        -- Code here runs when the scene is on screen (but is about to go off screen)
 
    elseif ( phase == "did" ) then
        -- Code here runs immediately after the scene goes entirely off screen
 
    end
end
 
 
-- destroy()
function scene:destroy( event )
 
    local sceneGroup = self.view
    -- Code here runs prior to the removal of scene's view
 
end
 
 
-- -----------------------------------------------------------------------------------
-- Scene event function listeners
-- -----------------------------------------------------------------------------------
scene:addEventListener( "create", scene )
scene:addEventListener( "show", scene )
scene:addEventListener( "hide", scene )
scene:addEventListener( "destroy", scene )
-- -----------------------------------------------------------------------------------
 
return scene
