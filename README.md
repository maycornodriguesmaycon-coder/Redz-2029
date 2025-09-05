		
		return Url
	end
	
	function fetcher.get(Url)
		local success, response = pcall(function()
			return game:HttpGet(formatUrl(Url))
		end)
		
		if success then
			return response
		else
			CreateMessageError(`[1] [{ identifyexecutor() }] failed to get http/url/raw: { Url }\n>>{ response }<<`)
		end
	end
	
	function fetcher.load(Url: string, concat: string?)
		local raw = fetcher.get(Url) .. (if concat then concat else "")
		local runFunction, errorText = loadstring(raw)
		
		if type(runFunction) ~= "function" then
			CreateMessageError(`[2] [{ identifyexecutor() }] sintax error: { Url }\n>>{ errorText }<<`)
		else
			return runFunction
		end
	end
end

local function IsPlace(Script)
	if Script.PlacesIds and table.find(Script.PlacesIds, game.PlaceId) then
		return true
	elseif Script.GameId and Script.GameId == game.GameId then
		return true
	end
end

for _, Script in Scripts do
	if IsPlace(Script) then
		return fetcher.load("{Repository}Games/" .. Script.UrlPath)(fetcher, ...)
	end
end
